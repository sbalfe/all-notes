# The Chilli Framework

- Block of memory in a buffer
  - We write values to this block of memory.
    - These are pixels
- When we are ready to show to user $\to$​ copy to texture
  - This is in GPU memory > render the texture to the frame buffer.

- Begin frame creates a clean buffer
- End frame copies to the  texture

---

- Matrices in direct are applied in the order they are stated rather than the other way around

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210801024123257.png)

----

- lose precision on each rotation.
- keep angles in 2pi range to keep precision.

---

https://docs.microsoft.com/en-us/windows/win32/direct3d11/d3d10-graphics-programming-guide-rasterizer-stage-rules - raster rules

```c++
/******************************************************************************************
*	Chili DirectX Framework Version 16.10.01											  *
*	Graphics.cpp																		  *
*	Copyright 2016 PlanetChili.net <http://www.planetchili.net>							  *
*																						  *
*	This file is part of The Chili DirectX Framework.									  *
*																						  *
*	The Chili DirectX Framework is free software: you can redistribute it and/or modify	  *
*	it under the terms of the GNU General Public License as published by				  *
*	the Free Software Foundation, either version 3 of the License, or					  *
*	(at your option) any later version.													  *
*																						  *
*	The Chili DirectX Framework is distributed in the hope that it will be useful,		  *
*	but WITHOUT ANY WARRANTY; without even the implied warranty of						  *
*	MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the						  *
*	GNU General Public License for more details.										  *
*																						  *
*	You should have received a copy of the GNU General Public License					  *
*	along with The Chili DirectX Framework.  If not, see <http://www.gnu.org/licenses/>.  *
******************************************************************************************/
#include "MainWindow.h"
#include "Graphics.h"
#include "DXErr.h"
#include "ChiliException.h"
#include <assert.h>
#include <string>
#include <array>
#include <functional>

// Ignore the intellisense error "cannot open source file" for .shh files.
// They will be created during the build sequence before the preprocessor runs.
namespace FramebufferShaders
{
#include "FramebufferPS.shh"
#include "FramebufferVS.shh"
}

#pragma comment( lib,"d3d11.lib" )

using Microsoft::WRL::ComPtr;

Graphics::Graphics( HWNDKey& key )
	:
	sysBuffer( ScreenWidth,ScreenHeight )
{
	assert( key.hWnd != nullptr );

	//////////////////////////////////////////////////////
	// create device and swap chain/get render target view
	DXGI_SWAP_CHAIN_DESC sd = {};
	sd.BufferCount = 1;
	sd.BufferDesc.Width = Graphics::ScreenWidth;
	sd.BufferDesc.Height = Graphics::ScreenHeight;
	sd.BufferDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;
	sd.BufferDesc.RefreshRate.Numerator = 1;
	sd.BufferDesc.RefreshRate.Denominator = 60;
	sd.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
	sd.OutputWindow = key.hWnd;
	sd.SampleDesc.Count = 1;
	sd.SampleDesc.Quality = 0;
	sd.Windowed = TRUE;

	D3D_FEATURE_LEVEL	featureLevelsRequested = D3D_FEATURE_LEVEL_9_1;
	UINT				numLevelsRequested = 1;
	D3D_FEATURE_LEVEL	featureLevelsSupported;
	HRESULT				hr;
	UINT				createFlags = 0u;
#ifdef _DEBUG
#ifdef USE_DIRECT3D_DEBUG_RUNTIME
	createFlags |= D3D11_CREATE_DEVICE_DEBUG;
#endif
#endif
	
	// create device and front/back buffers
	if( FAILED( hr = D3D11CreateDeviceAndSwapChain( 
		nullptr,
		D3D_DRIVER_TYPE_HARDWARE,
		nullptr,
		createFlags,
		&featureLevelsRequested,
		numLevelsRequested,
		D3D11_SDK_VERSION,
		&sd,
		&pSwapChain,
		&pDevice,
		&featureLevelsSupported,
		&pImmediateContext ) ) )
	{
		throw CHILI_GFX_EXCEPTION( hr,L"Creating device and swap chain" );
	}

	// get handle to backbuffer
	ComPtr<ID3D11Resource> pBackBuffer;
	if( FAILED( hr = pSwapChain->GetBuffer(
		0,
		__uuidof( ID3D11Texture2D ),
		(LPVOID*)&pBackBuffer ) ) )
	{
		throw CHILI_GFX_EXCEPTION( hr,L"Getting back buffer" );
	}

	// create a view on backbuffer that we can render to
	if( FAILED( hr = pDevice->CreateRenderTargetView( 
		pBackBuffer.Get(),
		nullptr,
		&pRenderTargetView ) ) )
	{
		throw CHILI_GFX_EXCEPTION( hr,L"Creating render target view on backbuffer" );
	}


	// set backbuffer as the render target using created view
	pImmediateContext->OMSetRenderTargets( 1,pRenderTargetView.GetAddressOf(),nullptr );


	// set viewport dimensions
	D3D11_VIEWPORT vp;
	vp.Width = float( Graphics::ScreenWidth );
	vp.Height = float( Graphics::ScreenHeight );
	vp.MinDepth = 0.0f;
	vp.MaxDepth = 1.0f;
	vp.TopLeftX = 0.0f;
	vp.TopLeftY = 0.0f;
	pImmediateContext->RSSetViewports( 1,&vp );


	///////////////////////////////////////
	// create texture for cpu render target
	D3D11_TEXTURE2D_DESC sysTexDesc;
	sysTexDesc.Width = Graphics::ScreenWidth;
	sysTexDesc.Height = Graphics::ScreenHeight;
	sysTexDesc.MipLevels = 1;
	sysTexDesc.ArraySize = 1;
	sysTexDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;
	sysTexDesc.SampleDesc.Count = 1;
	sysTexDesc.SampleDesc.Quality = 0;
	sysTexDesc.Usage = D3D11_USAGE_DYNAMIC;
	sysTexDesc.BindFlags = D3D11_BIND_SHADER_RESOURCE;
	sysTexDesc.CPUAccessFlags = D3D11_CPU_ACCESS_WRITE;
	sysTexDesc.MiscFlags = 0;
	// create the texture
	if( FAILED( hr = pDevice->CreateTexture2D( &sysTexDesc,nullptr,&pSysBufferTexture ) ) )
	{
		throw CHILI_GFX_EXCEPTION( hr,L"Creating sysbuffer texture" );
	}

	D3D11_SHADER_RESOURCE_VIEW_DESC srvDesc = {};
	srvDesc.Format = sysTexDesc.Format;
	srvDesc.ViewDimension = D3D11_SRV_DIMENSION_TEXTURE2D;
	srvDesc.Texture2D.MipLevels = 1;
	// create the resource view on the texture
	if( FAILED( hr = pDevice->CreateShaderResourceView( pSysBufferTexture.Get(),
		&srvDesc,&pSysBufferTextureView ) ) )
	{
		throw CHILI_GFX_EXCEPTION( hr,L"Creating view on sysBuffer texture" );
	}


	////////////////////////////////////////////////
	// create pixel shader for framebuffer
	// Ignore the intellisense error "namespace has no member"
	if( FAILED( hr = pDevice->CreatePixelShader(
		FramebufferShaders::FramebufferPSBytecode,
		sizeof( FramebufferShaders::FramebufferPSBytecode ),
		nullptr,
		&pPixelShader ) ) )
	{
		throw CHILI_GFX_EXCEPTION( hr,L"Creating pixel shader" );
	}
	

	/////////////////////////////////////////////////
	// create vertex shader for framebuffer
	// Ignore the intellisense error "namespace has no member"
	if( FAILED( hr = pDevice->CreateVertexShader(
		FramebufferShaders::FramebufferVSBytecode,
		sizeof( FramebufferShaders::FramebufferVSBytecode ),
		nullptr,
		&pVertexShader ) ) )
	{
		throw CHILI_GFX_EXCEPTION( hr,L"Creating vertex shader" );
	}
	

	//////////////////////////////////////////////////////////////
	// create and fill vertex buffer with quad for rendering frame
	const FSQVertex vertices[] =
	{
		{ -1.0f,1.0f,0.5f,0.0f,0.0f },
		{ 1.0f,1.0f,0.5f,1.0f,0.0f },
		{ 1.0f,-1.0f,0.5f,1.0f,1.0f },
		{ -1.0f,1.0f,0.5f,0.0f,0.0f },
		{ 1.0f,-1.0f,0.5f,1.0f,1.0f },
		{ -1.0f,-1.0f,0.5f,0.0f,1.0f },
	};
	D3D11_BUFFER_DESC bd = {};
	bd.Usage = D3D11_USAGE_DEFAULT;
	bd.ByteWidth = sizeof( FSQVertex ) * 6;
	bd.BindFlags = D3D11_BIND_VERTEX_BUFFER;
	bd.CPUAccessFlags = 0u;
	D3D11_SUBRESOURCE_DATA initData = {};
	initData.pSysMem = vertices;
	if( FAILED( hr = pDevice->CreateBuffer( &bd,&initData,&pVertexBuffer ) ) )
	{
		throw CHILI_GFX_EXCEPTION( hr,L"Creating vertex buffer" );
	}

	
	//////////////////////////////////////////
	// create input layout for fullscreen quad
	const D3D11_INPUT_ELEMENT_DESC ied[] =
	{
		{ "POSITION",0,DXGI_FORMAT_R32G32B32_FLOAT,0,0,D3D11_INPUT_PER_VERTEX_DATA,0 },
		{ "TEXCOORD",0,DXGI_FORMAT_R32G32_FLOAT,0,12,D3D11_INPUT_PER_VERTEX_DATA,0 }
	};

	// Ignore the intellisense error "namespace has no member"
	if( FAILED( hr = pDevice->CreateInputLayout( ied,2,
		FramebufferShaders::FramebufferVSBytecode,
		sizeof( FramebufferShaders::FramebufferVSBytecode ),
		&pInputLayout ) ) )
	{
		throw CHILI_GFX_EXCEPTION( hr,L"Creating input layout" );
	}


	////////////////////////////////////////////////////
	// Create sampler state for fullscreen textured quad
	D3D11_SAMPLER_DESC sampDesc = {};
	sampDesc.Filter = D3D11_FILTER_MIN_MAG_MIP_POINT;
	sampDesc.AddressU = D3D11_TEXTURE_ADDRESS_CLAMP;
	sampDesc.AddressV = D3D11_TEXTURE_ADDRESS_CLAMP;
	sampDesc.AddressW = D3D11_TEXTURE_ADDRESS_CLAMP;
	sampDesc.ComparisonFunc = D3D11_COMPARISON_NEVER;
	sampDesc.MinLOD = 0;
	sampDesc.MaxLOD = D3D11_FLOAT32_MAX;
	if( FAILED( hr = pDevice->CreateSamplerState( &sampDesc,&pSamplerState ) ) )
	{
		throw CHILI_GFX_EXCEPTION( hr,L"Creating sampler state" );
	}
}

Graphics::~Graphics()
{
	// clear the state of the device context before destruction
	if( pImmediateContext ) pImmediateContext->ClearState();
}

void Graphics::EndFrame()
{
	HRESULT hr;

	// lock and map the adapter memory for copying over the sysbuffer
	if( FAILED( hr = pImmediateContext->Map( pSysBufferTexture.Get(),0u,
		D3D11_MAP_WRITE_DISCARD,0u,&mappedSysBufferTexture ) ) )
	{
		throw CHILI_GFX_EXCEPTION( hr,L"Mapping sysbuffer" );
	}
	// perform the copy line-by-line
	sysBuffer.Present( mappedSysBufferTexture.RowPitch,
		reinterpret_cast<BYTE*>(mappedSysBufferTexture.pData) );
	// release the adapter memory
	pImmediateContext->Unmap( pSysBufferTexture.Get(),0u );

	// render offscreen scene texture to back buffer
	pImmediateContext->IASetInputLayout( pInputLayout.Get() );
	pImmediateContext->VSSetShader( pVertexShader.Get(),nullptr,0u );
	pImmediateContext->PSSetShader( pPixelShader.Get(),nullptr,0u );
	pImmediateContext->IASetPrimitiveTopology( D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST );
	const UINT stride = sizeof( FSQVertex );
	const UINT offset = 0u;
	pImmediateContext->IASetVertexBuffers( 0u,1u,pVertexBuffer.GetAddressOf(),&stride,&offset );
	pImmediateContext->PSSetShaderResources( 0u,1u,pSysBufferTextureView.GetAddressOf() );
	pImmediateContext->PSSetSamplers( 0u,1u,pSamplerState.GetAddressOf() );
	pImmediateContext->Draw( 6u,0u );

	// flip back/front buffers
	if( FAILED( hr = pSwapChain->Present( 1u,0u ) ) )
	{
		throw CHILI_GFX_EXCEPTION( hr,L"Presenting back buffer" );
	}
}

void Graphics::BeginFrame()
{
	sysBuffer.Clear( Colors::Red );
}


//////////////////////////////////////////////////
//           Graphics Exception
Graphics::Exception::Exception( HRESULT hr,const std::wstring& note,const wchar_t* file,unsigned int line )
	:
	ChiliException( file,line,note ),
	hr( hr )
{}

std::wstring Graphics::Exception::GetFullMessage() const
{
	const std::wstring empty = L"";
	const std::wstring errorName = GetErrorName();
	const std::wstring errorDesc = GetErrorDescription();
	const std::wstring& note = GetNote();
	const std::wstring location = GetLocation();
	return    (!errorName.empty() ? std::wstring( L"Error: " ) + errorName + L"\n"
		: empty)
		+ (!errorDesc.empty() ? std::wstring( L"Description: " ) + errorDesc + L"\n"
			: empty)
		+ (!note.empty() ? std::wstring( L"Note: " ) + note + L"\n"
			: empty)
		+ (!location.empty() ? std::wstring( L"Location: " ) + location
			: empty);
}

std::wstring Graphics::Exception::GetErrorName() const
{
	return DXGetErrorString( hr );
}

std::wstring Graphics::Exception::GetErrorDescription() const
{
	std::array<wchar_t,512> wideDescription;
	DXGetErrorDescription( hr,wideDescription.data(),wideDescription.size() );
	return wideDescription.data();
}

std::wstring Graphics::Exception::GetExceptionType() const
{
	return L"Chili Graphics Exception";
}

void Graphics::DrawLine( float x1,float y1,float x2,float y2,Color c )
{
	const float dx = x2 - x1;
	const float dy = y2 - y1;

	if( dy == 0.0f && dx == 0.0f )
	{
		PutPixel( int( x1 ),int( y1 ),c );
	}
	else if( abs( dy ) > abs( dx ) )
	{
		if( dy < 0.0f )
		{
			std::swap( x1,x2 );
			std::swap( y1,y2 );
		}

		const float m = dx / dy;
		float y = y1;
		int lastIntY;
		for( float x = x1; y < y2; y += 1.0f,x += m )
		{
			lastIntY = int( y );
			PutPixel( int( x ),lastIntY,c );
		}
		if( int( y2 ) > lastIntY )
		{
			PutPixel( int( x2 ),int( y2 ),c );
		}
	}
	else
	{
		if( dx < 0.0f )
		{
			std::swap( x1,x2 );
			std::swap( y1,y2 );
		}

		const float m = dy / dx;
		float x = x1;
		int lastIntX;
		for( float y = y1; x < x2; x += 1.0f,y += m )
		{
			lastIntX = int( x );
			PutPixel( lastIntX,int( y ),c );
		}
		if( int( x2 ) > lastIntX )
		{
			PutPixel( int( x2 ),int( y2 ),c );
		}
	}
}

void Graphics::DrawTriangle( const Vec2& v0,const Vec2& v1,const Vec2& v2,Color c )
{
	// using pointers so we can swap (for sorting purposes)
	const Vec2* pv0 = &v0;
	const Vec2* pv1 = &v1;
	const Vec2* pv2 = &v2;

	// sorting vertices by y
	if( pv1->y < pv0->y ) std::swap( pv0,pv1 );
	if( pv2->y < pv1->y ) std::swap( pv1,pv2 );
	if( pv1->y < pv0->y ) std::swap( pv0,pv1 );
	

	// if they share the same y coordinate v0 and v1, then it has a flat top or straight line at top
	if( pv0->y == pv1->y ) // natural flat top
	{
		// sorting top vertices by x, v0 must be on the left
		if( pv1->x < pv0->x ) std::swap( pv0,pv1 );
		DrawFlatTopTriangle( *pv0,*pv1,*pv2,c );
	}
	else if( pv1->y == pv2->y ) // natural flat bottom
	{
		// sorting bottom vertices by x
		if( pv2->x < pv1->x ) std::swap( pv1,pv2 );
		DrawFlatBottomTriangle( *pv0,*pv1,*pv2,c );
	}
	else // general triangle
	{
		// find splitting vertex
		const float alphaSplit =
			(pv1->y - pv0->y) /
			(pv2->y - pv0->y);
		const Vec2 vi = *pv0 + (*pv2 - *pv0) * alphaSplit;

		/*
			if x of splittig vertex is less than vertex of v1, then its clearly
		*/
		if( pv1->x < vi.x ) // major right
		{
			/* the order of the inputs here matters as the flat bottom and top*/

			/*
				refer to ipad drawings but this is the order of vertices from the top of the triangle
			*/
			DrawFlatBottomTriangle( *pv0,*pv1,vi,c );
			DrawFlatTopTriangle( *pv1,vi,*pv2,c );
		}
		else // major left
		{
			DrawFlatBottomTriangle( *pv0,vi,*pv1,c );
			DrawFlatTopTriangle( vi,*pv1,*pv2,c );
		}
	}
}

void Graphics::DrawFlatTopTriangle( const Vec2& v0,const Vec2& v1,const Vec2& v2,Color c )
{
	// calulcate slopes in screen space, run over rise , avoid infinite slope from the straight lines upwards
	float m0 = (v2.x - v0.x) / (v2.y - v0.y);
	float m1 = (v2.x - v1.x) / (v2.y - v1.y);

	// calculate start and end scanlines, start and end y coordinates to render in 
	const int yStart = (int)ceil( v0.y - 0.5f );
	const int yEnd = (int)ceil( v2.y - 0.5f ); // the scanline AFTER the last line drawn

	for( int y = yStart; y < yEnd; y++ )
	{
		// caluclate start and end points (x-coords)
		// add 0.5 to y value because we're calculating based on pixel CENTERS

		/*
			px0, we take the gradient of the left line and use to calculate how far we have moved from the 

			originall value of v0.x of which we are adding on here

			this calculation is done via first centering the scanline , y, by adding 1/2 , start from center of pixel

			there is some value of y for the top vertex so we must take the difference of our central coordinate to obtain the y distance

			this y distance and multiplication by gradient obtains the distance in x down that side, just add our initial x value to obtaint the final value

			same principles for px1.
		*/
		const float px0 = m0 * (float( y ) + 0.5f - v0.y) + v0.x;
		const float px1 = m1 * (float( y ) + 0.5f - v1.y) + v1.x;

		// calculate start and end pixels

		/* for the x start we take the ceiling to apply top left raster rule
			
			pixels are discrete values on our buffer to fill in the screen depending on window size

			this negation of 1/2 ensures we cut of pixel centers that are outside of the line
		*/
		const int xStart = (int)ceil( px0 - 0.5f );
		const int xEnd = (int)ceil( px1 - 0.5f ); // the pixel AFTER the last pixel drawn

		for( int x = xStart; x < xEnd; x++ )
		{
			PutPixel( x,y,c );
		}
	}
}

void Graphics::DrawFlatBottomTriangle( const Vec2& v0,const Vec2& v1,const Vec2& v2,Color c )
{
	// calulcate slopes in screen space
	float m0 = (v1.x - v0.x) / (v1.y - v0.y);
	float m1 = (v2.x - v0.x) / (v2.y - v0.y);

	// calculate start and end scanlines
	const int yStart = (int)ceil( v0.y - 0.5f );
	const int yEnd = (int)ceil( v2.y - 0.5f ); // the scanline AFTER the last line drawn

	for( int y = yStart; y < yEnd; y++ )
	{
		// caluclate start and end points
		// add 0.5 to y value because we're calculating based on pixel CENTERS
		const float px0 = m0 * (float( y ) + 0.5f - v0.y) + v0.x;
		const float px1 = m1 * (float( y ) + 0.5f - v0.y) + v0.x;

		// calculate start and end pixels
		const int xStart = (int)ceil( px0 - 0.5f );
		const int xEnd = (int)ceil( px1 - 0.5f ); // the pixel AFTER the last pixel drawn

		for( int x = xStart; x < xEnd; x++ )
		{
			PutPixel( x,y,c );
		}
	}
}
```

---

#### rasterization process summary.

![illustration of examples of top-left triangle rasterization](https://docs.microsoft.com/en-us/windows/win32/direct3d11/images/d3d10-rasterrulestriangle.png)

- Okay so first find our vertices of the triangle, 3 points.
- We must decided what pixels to draw , as there may be multiple connected vertices in the triangles.

---

#### Draw triangle

- Pass in 3 vertices.
- sort the vertices so that the largest y is nearest the bottom of the vertex pointers.
- If the coordinates of the bottom most ones are the same , bottom from a 2D perspective starts in **top left corner**
  - then we have **flat top triangle**
- if the coordinates of the top 2 are the same then its **flat bottom triangle**
- other we must calculate a **split vertex** to determine **major right or major left**.

##### Splitting vertex

- This is just a ratio between the p1 point and the point it connects with on the other end

  - find distance between 0 and 1 vertex, divided by vertex 2 distance and vertex 0 distance
    - This creates a ratio, cutting point on the side of v2 > v0,

  $\Huge{\text{split}= \frac{p_{1y} - p_{0y}}{p_{v2y} - p_{v0y}}}$ 

- find the vertex by $(1-\alpha)\cdot v_{0} + \alpha v_2$

- Example if the split was perfectly half way 

  - 0.5 x vertex 0 + 0.5 v2 making it halfway between that line, its just a value to determine between the line to decide where to cut , using the ratio.

- if the x value of v1 is less than vertex splitting x it must be a major right.
  - otherwise **major left**

---

**Drawing major right**

- draw the flat bottom with the vertices in order, 0, v1, split 
- draw the flat top with vertices order, v1, split, v2

**drawing flat bottom**

- calculate gradient of both sides
- calculate start and end of the scanlines
  - this is just the ceiling of the y coordinate of v0 and v2 y value
    - scanlines are discrete y values to start drawing pixels from, ceil(v0.y - 1.2) > take off half as its a left hand rule at the center
  - does not include the end

- Draw a for loop from these scanlines , like cycling over the y 
  - calculate the x coordinates from where to start
    - This is just a basic gradient x (y + 0.5f (centre of pixel) + inital y) * intial x
      - essentially first part find how far the x is gone down + the x that was already there
- we then choose the start and end pixels
  - start , via top left rule is the calculated x0 - 1/2  with ceiling taken to obtain an integer, if the center its at is 
    - This just essentially locks the start to the edge of a pixel to start drawing from left to right
- then just loop this value placing a pixel in each value , along with the color.

---

## Textures

### FlatTopTriangleTex

- init texture coordinates
  - edge left = v0.t

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210803002747497.png)

- Shows how the top and bottom are fucked up as you are only querying red and blue for them as the other vertices have been used up alread.y

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210803194157871.png)

top to bottom = 0,3 (4) v 

left to right = -1, 2 (3) u



u right , v down

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210803194454498.png)

---

### Pixel Shader

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210804084741140.png)

- fixed colours, just doing a texture lookup.

http://www.lysator.liu.se/~mikaelk/doc/perspectivetexture/ - prove of perspective correction linear space 

https://www.scratchapixel.com/lessons/3d-basic-rendering/rasterization-practical-implementation/perspective-correct-interpolation-vertex-attributes - perspective correction in detail.

https://computergraphics.stackexchange.com/questions/8481/why-do-perspective-correction-based-texture-mapping-do-depth-division

basic idea

> What the equation says is that to interpolate a vertex attribute correctly, we first need to divide by the vertex attribute value by the z-coordinate of the vertex it is defined to, then linearly interpolate them using q (which in our case, will be the barycentric coordinates of the pixel on the 2D triangle), and then finally multiply the result by ZZ, which is the depth of the point on the triangle, that the pixel overlaps (the depth of the point in camera space where the vertex attribute is being interpolated).

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210804112259488.png)

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210805120111313.png)

- Overview of our pipeline at tutorial 13 to show all the shaders and there posistion
- frag shader between triangle rasterization and put pixel
- vertex shader between vertices list and triangle assembly 
- finally , geometry shader between triangle assembly and screen view port transform
  - Vertex shader part of mesh, could change an entire mesh
    - Geometry shader rather just alters the triangles as they are.

- can be used to generate the face normal of every triangle.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210805124317479.png)

##### Direct3D / OpenGL differences in geo shader

- its optional

- triangle assembler in ours passes the indexes.

- can pass into the pixel shader if not geometry shader

- geometry shader in those can alter the primitive types

  ​	1. point , line, strip etc.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210805124500696.png)

- example
  - for sprites > we can pass an id to the vertex shader
  - this id when reaching the geometry shader can query the vertices for that sprite in a look up table
  - and output a textured quad for example. 
  - all from just single points inputs > lowers the overall bandwidth in the pipeline

- Can take in a few triangles and create many more triangles from that as a result
  - Amplifies the geometry
    - Can reject triangles also.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210805125117448.png)

- As you can see it has access to adjacency information
- Issue with its , its very slow 
  - Amplification.
- Tessellation is a useful feature but still very slow.
- Used in shadow mapping.
- directX have tessellation shader amon

---

# Loading meshes

- Point light > the angle of point light decreases as you go further from the image sensor
  - This creates horizontal light eventually

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210805193340711.png)

- Cant be that far away compared to the scene.
- directional light are parallel in a set direction unlike point light

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210805193424997.png)

- here light initially scatters perfectly as the surface is aligned properly.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210805193500448.png)

- May enter eye of the viewer > this surface becomes visible
- brightness of the board depends on
  1. Light intensity

---

- Reduce density hitting board by angling the board

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210805193620745.png)

- Appears dimmer.
- Side on $\to$​ makes it appear **black**.

---

- Can perceived depth and shading on a 3d model using this concept of light hitting at certain angles.

---

https://en.wikipedia.org/wiki/Bounding_sphere - bounding 

---

# Gourad shading

- Must make some smooth.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210806133728287.png)

- Use latitude and longitude method find the vertices of the sphere 

- Double for loop between each long and latitude. 

---

Normal maps > apply a normal map like a texture to add detail to a low poly model to add depth when shading. 

## 4 element vectors / Matrices

-  Combine translations to one matrix with an extra coordinate and larger matrix rather than apply both.

---

## Projection

Javid explanation of near and far matrix projection value 

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210808104214638.png)

- Scale The system to normalized device coordinates
- $\text{ZFar - ZNear}$​​​ = 10- 1 = 9 , take our point and divided by 9 first > obtain point between 0 and 1
- Must scale back to the far plane by 10 in this case now of course giving us  $\Large\frac{\text{Zfar}}{\text{ZFar - ZNear}}$​

---

- This still eaves us with the gap between camera and the near clipping plane. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210808104618492.png)

- multiplying the value but instead by Znear also to get the near clip plane value added 
- this is offset from the total value to remove it essentially from the calculation of normalization
- we dont want to  include between this space just between far and near clipping planes.

https://www.youtube.com/watch?v=1yQcXsC4diY

![The Viewing Frustum](https://lh3.googleusercontent.com/proxy/D2ISz7SiFlOP3W_XsBFL3nD33qHZ0TOCSTqUiHWncYINYGHEoo-khkiaQN1_1JJYv3C9npyPryCoBSkFI_8YgoFgyC7n26bAWT_9mF8)

- use near * tan(FOV/2) calculation to obtain the value for x' on increase in angle
- proper map is x > D * tan(fov /2) to obtain x'
- remember that other part to the z value is a translation ammount.
- so we scale the point down to the near plane and then negate the final ammount to properly map the values for z part of perspective matrix.

https://stackoverflow.com/questions/46799855/why-is-the-opengl-projection-matrix-needlessly-complicated

