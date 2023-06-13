| Keyword                      | Use                                                          | Notable Attributes/features                                  | Further links                                                |
| ---------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Style                        | used inline html to create css attributes without an external file<br>Can create internal css at top of file by defining certain tags within it in order to affect each of the tags<br>must be within the <head> | background-colour: , sets the background colour for the tag within which you intend - can use the hex code value. |                                                              |
| background-colour.           | takes in a value such as hex/text/RGB values to change the colour of the section of the web page.- |                                                              | https://www.w3schools.com/cssref/pr_background-color.asp     |
| Border style                 | Changes the body style of the element                        |                                                              | https://developer.mozilla.org/en-US/docs/Web/CSS/border-style |
| height                       | in px, uses to change the height                             | can use a percentage instead to mean relative size of the page such as stay 30% of parent | https://www.w3schools.com/cssref/pr_dim_height.asp           |
| width                        | in px, uses to change width.                                 | same percentage idea as with height                          |                                                              |
| border-top-style             | replace top with bottom left right to style the border on each side |                                                              |                                                              |
| border-width                 | change the border width in px                                |                                                              |                                                              |
| none                         | used in border-style to set the side of the element to be ignored when applying a style property to it. |                                                              |                                                              |
| color                        | change text colour within a certain tag                      |                                                              |                                                              |
| margin-top,left,bottom,right | Change the ammount of margin on each side of this elements box within the css |                                                              |                                                              |
| Display                      | Change the display value of a tag to : block/inline/inline block or none. |                                                              |                                                              |
| Visibility                   | can be set to hidden in order to remove an element but keep its position and the elements around it still shape as if that element were still present.<br /> |                                                              |                                                              |
| Position                     | static,relative,absolute,fixed<br>Contains the top left bottom and right values to determine which way to apply this positioning |                                                              |                                                              |
|                              |                                                              |                                                              |                                                              |
|                              |                                                              |                                                              |                                                              |

## Notable CSS links

https://colorhunt.co/

https://developer.mozilla.org/en-US/docs/Web/CSS/color_value

https://www.w3schools.com/cssref/css_default_values.asp

https://developer.mozilla.org/en-US/docs/Web/CSS/border-style

https://www.w3schools.com/html/html_css.asp

https://developer.mozilla.org/en-US/docs/Web/CSS/Reference - MAIN REFERENCE

https://developer.mozilla.org/en-US/docs/Web/CSS/margin - margins

https://www.cssfontstack.com/ - css safe fonts 

https://fonts.google.com/ - google fonts

https://www.flaticon.com/ - nice images for websites. 

gifs -https://giphy.com/

```css
   <!-- internal css -->
    <style>

        /* This is a CSS comment */

        /* Changes the background colour for all body tags */

        body {

            background-color:lightskyblue;

        }

        /* No effect as their is some default css applied to html already */
        hr {
        
            border-style:none;
            border-top-style:dotted;
            width:5%;
            border-color:rgb(207, 203, 203);
            border-width:5px;
        }

        
    </style>
```

##### Link up a css style sheet. 

```css


    <!-- link up a css styl sheet using the referecne location in href-->>
    <link rel = "stylesheet" href = "CSS/styles.css">

```

- this can be placed in any head of a html file to apply the same styles and you can change values of css within that and affect every single page at once. 

  

##### Debugging CSS

In chrome go to developer tools

- go to console and it will display any errors that are present in the current html and css.

- Inline CSS has greater precedence than the external style sheets therefore overrides any css written there, also has greater precedence than the internal css within the head.

### Anatomy of css syntax

#### **selector {property: value;}**

- always ends with semicolons
- selector is the **who** $\to$ who do you want to modify in the web page. 
- property is the **what** $\to$ what do you want to change
- **how** is the value $\to$ how do you want to change this property.

#### Class selectors

- enter a unique name for the actual tag 

```html
<img class = "bacon" src="https://emojipedia-us.s3.amazonaws.com/thumbs/240/apple/118/bacon_1f953.png" alt="bacon-img">

  <img class = "broccoli" src="https://emojipedia-us.s3.dualstack.us-west-1.amazonaws.com/thumbs/120/apple/271/broccoli_1f966.png" alt = "broccoli-img">
```

- Then in the css file , style each specific image using a dot before its class name instead img:
- class selectors **override** the tag selectors. 
-  Can be applied to **multiple tags.**

```css
/* Class selectors */

.bacon{

    background-color:green;

}

.broccoli{

    background-color:red;

}


```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20201111032411125.png)

- can have multiple class selectors for one single tag

```html
  <img class = "broccoli circular" src="https://emojipedia-us.s3.dualstack.us-west-1.amazonaws.com/thumbs/120/apple/271/broccoli_1f966.png" alt = "broccoli-img">
```

```css
.broccoli{

    background-color:red;

}

.circular{

    border-radius:100%;

}
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20201111033440824.png)

#### ID selectors

- define with a **#** and define within a tag as id = "etc"

```html
<h1 id = "heading">I Love Bacon</h1>
```

```css
#heading{

    background-color:blue;

}

```

- overrides the tag selectors definition. 

- An id selector is **unique** to where it is place and cannot be applied to multiple elements. 

- often used for something that is **unique** to a certain page and not duplicated at all. 

- Cannot have more than one selector within a single id and will return the entire id as void if you intend to do it this way. 

#### Pseudoclasses

- can change the css applied to a tag dynamically.
- example use with a tag selector by adding :hover 

```css
img:hover {

    background-color:gold;

}
```

- This changes the background colour to gold when hovering over it. 