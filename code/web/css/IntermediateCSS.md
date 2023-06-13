## Favicons

- little icon in the tab bar to display .

https://www.favicon.cc/

## Using DIV

- When dividing a section there is default html you can edit the default values by selecting it in chrome developer tools and altering pre inserted margin values for example. 

##### Margins

margin-top -> is the top attribute

## CSS box model

- Each object is a box that has properties of its size that can be changed
- Altering width and height pushes other containers around it. 

##### padding 

- like an internal margin of the items inside the container to the border of the container
- Actually increases the size of the container

##### margin

- the box that goes around an element outside of its border and container to show how much space is around it to where other elements can not reach.
- Acts like some buffer zone.
- does not increase the size of the container

##### border

- border element does not affect the size , its just what it says
- Give the box dimensions in a circle such as 0px top 10px right etc. 

![CSS Box Model](https://www.csssolid.com/images/box-model/css-box-model.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20201125052912684.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20201125052931617.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20201125052954987.png)

## CSS display Property

##### Block

- Take up the **entire width of the screen**
- Such as the text below which extends to the end. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20201205195945476.png)

- <paragraph tag> is a **block element** $\to$ they cannot be joined in one line to create seperate words in order to style separately. 

##### Inline

- Only takes up the space that is required for it 
![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20201205200647332.png)

##### Inline-Block

- Can make block elements
- You now change the width of the elements. 
- They get displayed next to each other with the desired size. 

##### None

- Removes the element from the page

  

## CSS Positioning

- Content determines the size of something
- The order of tags determines everything as default
- The **children** sit **on top of parents** such as a h1 within a div , this acts as **a z axis**.

---

### Static

- The default position

### Relative

- Moves the element to be relative difference from the static position.
- Example relative position 30px places 30px from the image to the side of the left thus pushing it 30px to **the right** instead

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20201214051945795.png)

- This does **not affect** any other elements within on the page. 

### Absolute Position

- An element in a parent container
- Shifts relative to the parent container rather than its ghost image.
- Example would be absolute 30px in right direction , this would move to the righthand side of the parent and push 30px to the left
  - so it would be 30px off the right hand side of its parent.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20201214052709912.png)

- Same as relative but its for the **parent element**.

- The elements react to an item being absolutely moved
  - They would fill space it took up if moved about 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20201214052915450.png)

- Blue and yellow moved up as red got absolutely positioned from the left hand of the parent by 200px. 

### Fixed

- When you set a position the element willy stay in the same position no matter how far you scroll down the website 
- Useful for some navigation or sidebar element within the site.

### The Z axis and stacking order

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20201220210504855.png)

setting red z index to 1 to come to the front

The default z index of elements is set to 0.

when the z index is the same it reverts to the precedence of parent containers or order of div elements.

This only works when a position property is applied such as relative or absolute

```html
<div class = "red">
    red
</div>    
<div class = "yellow">
    yellow
</div>    
<div class = "blue">
    blue
</div>    

<!--There are 3 things to decided order of stacking-->

<!--1. Children within parents e.g. div inside div > child is on top of parent-->

<!--2. Order of writing down on the page > higher up ones appear further back-->

<!--3. Z index decides the order where they are all default at 0, for this to apply an element must be using-->
<!--a position property-->
```

```css
div{
    
    height:100px;
    width:100px;
    
    border: 1px solid;
    
    
}

.red{
    
    background-color: red;
  
    
    position: absolute;
    
}
.yellow{
    
    background-color: yellow;
    left: 20px;
    top: 20px;
    z-index: 10;
    position: relative;
}

.blue{
    
    background-color: blue;
    left: 40px;
    z-index:1;
    
    
}
```

order of stacking

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20201220214604001.png)

last css rule always has priority in the braces. 

use ID just for sections of a page as its unique. 