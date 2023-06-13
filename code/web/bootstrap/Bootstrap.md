# Bootstrap

- Contains pre made classes for styling css quickly for a website. 
- Their is a bootstrap css pre built templates to work on in the examples folder. 

useful links



https://fontawesome.com/ - for images.

CDN:

```html
<script src="https://kit.fontawesome.com/6479332aa5.js" crossorigin="anonymous"></script>
```

- To use icons go the icons bit at top select an icon then it contains the code to access it from this file. 

##### CDN

- Content delivery network
- A variety of networks that can deliver a certain website. 



use this line to use bootstrap

```html


  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/css/bootstrap.min.css" rel="stylesheet"

   integrity="sha384-giJF6kkoqNQ00vy+HMDP7azOuL0xtbfIcaT9wjKHr8RbDVddVHyTfAAsrekwKmP1" crossorigin="anonymous">

put in the head of a html file. 


```



```html
<script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.5.4/dist/umd/popper.min.js" integrity="sha384-q2kxQ16AaE6UbzuKqyBE9/u/KzioAlnx2maXQHiDX9d4/zp8Ok3f+M7DPm+Ib6IU" crossorigin="anonymous"></script>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/js/bootstrap.min.js" integrity="sha384-pQQkAEnwaBkjpqZ8RU1fF1AKtTcHJwFl3pblpTlHXybJjHpMYo79HY3hIi4NKxyj" crossorigin="anonymous"></script>
```

### Wireframing

- Settle on a design before you start to add proper css. 
- https://dribbble.com/  - use for design insipiration
- https://sneakpeekit.com/ - useful wireframing tool or https://balsamiq.cloud/spaces - better



https://getbootstrap.com/docs/5.0/getting-started/introduction/ - Documentation



navbar hamburger menu example bootstrap

```html

<!--creates a navbar

	makes it so it expands to the proper size when the screen is large which means the hamburger button goes away

	navbar-dark themes the bar black fonts

	bg-dark sets the background of this bar dark
-->
<nav class = "navbar navbar-expand-lg navbar-dark bg-dark ">
    
    <!-- This defines the hamburger button drop down code when the screen is resized, uses external js / jquery to run it -->
     <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarTogglerDemo01" aria-			                 controls="navbarTogglerDemo01" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
    </button>
    
    <!-- This bootstrap defines the data within the drop down or when the screen is enlarged-->
    <div class = "collapse navbar-collapse" id  = "navbarTogglerDemo01">
        <!-- The order of stuff matter placing this navbar brand above the button makes it on the left -->
        <a class = "navbar-brand" href = "">Balls</a>
        <!-- ml auto it what makes the links move to the left below the navbar brand-->
        <ul class = "navbar-nav ml-auto" >
            
            <!-- nav items with the three links within them. -->
            <li class = "nav-item" >
                <a class = "nav-link" href="">Contact</a>
            </li>
            <li class = "nav-item" >
                <a class = "nav-link" href="">Pricing</a>
            </li>
            <li class = "nav-item" >
                <a class = "nav-link" href="">Download</a>
            </li>
        </ul> 
    </div>
</nav>
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20201216074413096.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20201216074425555.png)

- responsiveness refers to the viewport changes when elements move about on different technologies such a phone or an ipad. 

```html
<!-- Define a boostrap row -->
<div class = "row">
    
    <!-- 
    
        class col will divide the row equally to evenly distribute each of the columns
    
         having two divs will make two items taking up  a 50% proportion.
         
    -->
    
    <div class = "col" style = "background-color:red; border: 1px solid;">
        
        col
        
    </div>
     <div class = "col" style = "background-color:red; border: 1px solid;">
        
        col
        
    </div>
        
</div>


<div class = "row">
    
    <!-- The rows are divided into 12 lenght units therefore col-6 is 50% 
    
    
        its relative to 12 therefore col-3 will be 25%
    
        filled up all 12 sections in this below

    
    -->
    <div class = "col-6" style = "background-color:green; border: 1px solid">
        col-6
    </div>
     <div class = "col-6" style = "background-color:green; border: 1px solid">
        col-6
    </div>
    
</div>    


<div class = "row">
    
    <!-- 
    
    This defines that smartphones will have a full row whereas anythign medium or more will have 6 units / 50%
    They may resort to moving down a row on phone verion as it ran out of space when it was resized.
    
     md is for medium
    
    
    <div class = "col-md-6" style = "background-color:yellow; border: 1px solid;">
        col-md-6
    </div>    
      <div class = "col-md-6" style = "background-color:yellow; border: 1px solid;">
        col-md-6
    </div>  
    
    -->
    
    <!--
    
        col-lg-3 sets the size of each unit to take up 3 spaces when the viewport is of a large size
        
        col-md-4 sets teh size of each unit to be a 1/3 when the screen size is a medium size , roughly ipad, 4 fours thus pushes one row downwards.
        
        col-sm-6 when on phone the rows now take up only 50% of the row
    
    -->
    
    <div class = "col-lg-3 col-md-4 col-sm-6" style = "background-color:yellow; border: 1px solid;">
        col-lg-3
    </div>  
    
    <div class = "col-lg-3 col-md-4 col-sm-6" style = "background-color:yellow; border: 1px solid;">
        col-lg-3
    </div>  
    
    <div class = "col-lg-3 col-md-4 col-sm-6" style = "background-color:yellow; border: 1px solid;">
        col-lg-3
    </div> 
    
    <div class = "col-lg-3 col-md-4 col-sm-6" style = "background-color:yellow; border: 1px solid;">
        col-lg-3
    </div> 
    
</div>    
```

Containers

```html

<!-- Create a bootstrap container with the colour red  


    This container is automatically responses and changes its size with 
    the viewport.


-->

<div class = "container" style = "background-color:red;">
        
        Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt 
        ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco
        laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in 
        voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat
        non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

</div>

<!--
    
    fluid containers fill the entire length of the screen no matter what viewport. 

-->

<div class = "container-fluid" style = "background-color:yellow;">
        
        Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt 
        ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco
        laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in 
        voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat
        non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

</div>
```

#### Carousels

https://getbootstrap.com/docs/4.0/components/carousel/

```html


<!--Create a carousele element that cylces through various slides 


    in the main element there are various elements to create a functioning carousel 
    such as boolean to say should the user cycle or change the time interval
    refer to the documentation,
    
    data ride decides how it switches between elements in default its sliding but it can be determined by user input.

-->

<div id="testimonial-carousel" class="carousel slide" data-ride="carousel" data-ride = false>
    
  <!--Create main section of the carousel to hold each item in the carousel-->
  
  <div class="carousel-inner">
      
    <!--Item to be displayed first in the carousel this cycles through each element labelled carousel item-->
    
    <div class="carousel-item active" style = "background-color: red ;">
      <img class="d-block w-100" src="..." alt="First slide">
    </div>
    <div class="carousel-item" style = "background-color: green;">
      <img class="d-block w-100" src="..." alt="Second slide">
    </div>
    <div class="carousel-item" style = "background-color: yellow ;">
      <img class="d-block w-100" src="..." alt="Third slide">
    </div>
  </div>
  
  <!--Control Buttons-->
  
  <!--href to target the carousel to affect
  
    role = button turns the anchor tag to a button
    
    data slide determines direction
    
    carousel control prev icon is the class that displays the actual arrow built into bootstrap
    
    aria hidden is for display readers to stop them from reading out the text that is within these elements. 
    
    sr only is optional content for the assisitive technology. 
        
  -->
  
  <a class="carousel-control-prev" href="#testimonial-carousel" role="button" data-slide="prev">
    <span class="carousel-control-prev-icon" aria-hidden="true"></span>
    <span class="sr-only">Previous</span>
  </a>
  <a class="carousel-control-next" href="#testimonial-carousel" role="button" data-slide="next">
    <span class="carousel-control-next-icon" aria-hidden="true"></span>
    <span class="sr-only">Next</span>
  </a>
  
</div>
```

snippets of pre written bootstrap - https://bootsnipp.com/

card

```html
<div class = "card">
    
    <!--All card classes consist of these 3 sections-->
    
    <div class = "card-header">
        Card Header
    </div>    
     <div class = "card-body">
        Card Body
    </div>
     <div class = "card-footer">
        Card Footer
    </div>
    
</div>    
```

