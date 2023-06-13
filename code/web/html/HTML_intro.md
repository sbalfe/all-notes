

[](Contactme.html)

# Introduction to HTML

##### Useful links 

- [HTML MDN](https://developer.mozilla.org/kab/docs/Web/HTML) - Html Documentation

- [HTML CHEAT SHEET](https://web.stanford.edu/group/csp/cs21/htmlcheatsheet.pdf)
- [DevDocs for any language](https://devdocs.io/html/) 

```html

<!-- This is a html comment -->

<h1>This is a heading 1 </h1>
<h2>This is a heading 2 </h2>
<h3>This is a heading 3 </h3>
<h4>This is a heading 4 </h4>
<h5>This is a heading 5 </h5>
<h6>This is a heading 6 </h6>


```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20201022233654467.png)

- Tag omission - some tags can have an effect without the need to be closed, these are self closing tags.
- HTML attribute - extensions to tags that add extra detail that are included in the angle brackets. 
- Example Attributes for <!-- <hr> -->:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20201022234223175.png)

- We have added the size and noshade attribute to the code below. 

```html
<!-- Self closing tags -->

<!-- Horizontal rule , to create a straight line -->

<!-- centers all the html rendered between the tags -->

<center>
<hr>

<h1>The Adventures of <br> Sherlock Holmes</h1>

<!-- line break -->
<br>
<h3>by</h3>
<br>

<h2>SIR ARTHUR CONAN DOYLE</h2>

<hr size = "3" noshade> <!-- Looking at the documentation we can work this out -->
</center>
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20201023000238394.png)

- Boiler plate code
- Includes utf-8 to ensure the html site will render on many sets of symbols. 
- Other boiler plate codes give info about the author / viewport / description etc. 
- Description in the boilerplate can be used by google to locate websites. 

##### 

```html

<!-- Boiler Plate Basic HTML example -->>
<!DOCTYPE html> <!-- which version of html are we using , html5 -->
<html>
    <!-- head tag is the information at the top of the page in the tab -->
    <head>
        <!-- give extra meta data to the browser that is concerned with this file. 
			to render the characters correctly 
            utf -8 includes the unicode charater set which has many lanuages -->
        
        <meta charset =  "utf-8">
        <title></title>
    </head>
</html>

```

#### My personal site

##### index.html

```html
<!-- Boiler Plate Basic HTML example -->
<!DOCTYPE html>
<html>

<head>

    <meta charset="UTF-8">
    <title>Simon's Personal Site</title>

</head>

<body>

    <!-- organise the site using table layout placing the image on the RHS and information on the LHS by using seperate table data cells. -->>
    <table cellspacing = 12>
        <tr>
            <td><img src="images/emilia.gif" alt = "hot emilia"></td>

            <td> 
                <!-- paragraph tag -->

                <!-- strong for bold and em for italics-->

                <!-- anchor tag with href to the desired location -->
                
            <h1>Simon Balfe</h1><p><em>A fucking <strong>failure </strong>

            <a href="https://www.pornhub.com/">My site</a></em></p><p> I have no attributes, i hate myself, i want to die</p>

            </td>

        </tr>
    </table>

    <hr>

    <h3>Education</h3>

    <!-- unordered list -->
    <ul>
        <!-- List items-->

        <li>Balls</li>
        <li>Cocks</li>
        <li>Willy</li>

    </ul>

    <hr>

    <h3>Hobbies</h3>

    <!-- ordered list-->
    <ol>
        
        <li><a href = "https://www.youtube.com/">Stuff </a>nothing</li>
        <li>nothing</li>
        <li>nothing</li>

    </ol>

    <hr>

    <h3>Work Experience</h3>

    <!-- Tables add structure, this has two rows with the number of items horizontally equal to number <td>-->
    <table border = 1>
        <!-- header -->
        <thead>
            <tr>
                <th>Dates</th>
                <th>Work</th>
            </tr>
        </thead>

        <!-- Body -->
        <tbody>
             <tr>
            <td>2010-2013 </td>
            <td>Lead Developer at Piss Corporation</td>
        </tr>
        <tr>
            <td>2010</td>
            <td>Researcher at yes</td>
        </tr>
        </tbody>

        <!-- Footer -->
        <tfoot>
        </tfoot>

    </table>

    <!-- Skills table where there is a single row containing two data items of which both are tables themselves-->

    <h3>Skills</h3>

    <table border = 1>
        <tr>
            <td>
                <table>
                    <tr>
                        <td>Overwatch</td>
                        <td>&#11088 &#11088 &#11088 &#11088 &#11088</td>
                    </tr>
                    <tr>
                        <td>Death</td>
                        <td>&#11088 &#11088 &#11088 &#11088</td>
                    </tr>
                </table>
            </td>
            
            <td>
                <table>
                    <tr>
                        <td>Lol</td>
                        <td>&#11088 &#11088  &#11088</td>
                    </tr>
                    <tr>
                        <td>Death 2</td>
                        <td>&#11088 &#11088  </td>
                    </tr>
                </table>
            </td>
        </tr>
    </table>

    <hr>

    <a href ="hobbies.html">my hobbies</a>
    <a href = "ContactMe.html">Contact me</a>

</body>

</html>
```

##### Hobbies.html

```html
<!DOCTYPE html>

    <html>

    <head>
        <meta charset="utf-8">
    </head>

    <body>
        <!-- ordered list-->
        <ol>

            <li><a href="https://www.youtube.com/">Stuff </a>nothing</li>
            <li>nothing</li>
            <li>nothing</li>

        </ol>

    </body>

    </html>

    <html> 
```

##### Contactme.html

```html


<!DOCTYPE html>

    <html>

    <head>
        <meta charset="utf-8">
    </head>

    <body>
        <h1>My contact details</h1>
        <p>My fictional address</p>
        <p>047683409</p>
        <p>balls@cum.gmail.com</p>

        <hr>

        <!-- Action of malito:info@ opens up the default mail app to send the text to a certain email-->
        <form action ="mailto:info@sbmain17@gmail.com" method = "post" enctype = "text/plain">

            <!-- Create a label to tell the user what to enter-->
            <label>Your Name: </label>

            <!-- Create standard text input -->
            <input type = "text" name ="" value=""><br>
          
            <label> Your Email: </label>

            <!-- Does check to see if its a correct email format -->
            <input type = "email" name = ""><br>

            <label>Enter your message</label><br>

            <!--Create an input area for the user to type into-->
            <textarea area = "name" rows = "10" columns = "30"></textarea><br>

            <!-- Creates a submit button -->
            <input type = "submit" name ="">

        </form>

    </body>

    </html>
```
##### inputs.html

```html
  <form class = "" action ="index.html" method = "post">

            <!-- Create a label to tell the user what to enter-->
            <label>Your Name: </label>

            <!-- Create standard text input -->
            <input type = "text" name ="" value="">

            <!-- Creates a submit button -->
            <input type = "submit" name ="">

            <!-- Creates a Colour picker-->
            <input type = "color" name =""><br>

            <label>Do you want to sign up the email list?</label>

            <!-- Creates a checkbox tick -->
            <input type = "checkbox" name=""><br>

            <label> Enter your Password </label><br>

            <!-- Create a password input, default is masked-->
            <input type = "password" name = ""><br>

            <!-- file input-->
            <input type = "file" name = ""><br>

            <!-- Date input-->

            <input type = "date" name = ""><br>

            <!-- Radio button input-->
            <input type = "radio" name = ""><br>

            <!-- Range selector-->
            <input type = "range" name = "">

            <!--- Creates an email validation box-->>
            <input type = "email" name = "">

</form>

```

To sele