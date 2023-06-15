9.2 FOCG - Convolution

- Convolution underlies the **algorithms** used in:
  1. Sampling
  2. filtering
  3. reconstruction
- It is the **basis** of how we **analyze** the **algorithms** later

---

- Conv is an **operation** on **functions**
  - Takes **2 functions** $\to$ combines $\to$ produce **new function**
    - Operator denoted via $\star$ 
    - $f\star{g} $ $\to$ $f$ is **convolved** with $g$ and $f\star{g}$ is the **convolution**.

---

- May apply to **continuous / discrete sequences**
- Can be applied to **functions** defined on **1D** / **2D** and beyond **domains**
  - Functions of **two or more arguments**
- Start explanation with **discrete 1D cases first**
  - Continue to **continuous functions** and *two / three dimensions*

---

- In practice **functions** have **endpoints** rather than being **endless**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211016190745284.png)

##  Moving averages

- Obtain basic **picture** of some **convolution**
  - Consider **smoothing** a **1D function** using a **moving average**
    - To obtain some **smoothed value** at **any point** $\to$ compute **average** of the function over some **range** extending distance $r$ **each direction**
      - $r$ (*radius*) $\to$ is the **radius** of the **smoothing operation**
        - Its a **parameter** to **control** **the breadth** of **smoothing**

---

- State this **mathematically** for some **discrete / continuous functions**
  - Smoothing a **cont** function $g(x)$ $\to$ averaging means **integrating** $g$ over some **interval** 
    - Then **dividing** by the **length** of the **interval**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211016191133468.png)

- Alternatively, smoothing some **discrete function** $a[i]$
  - Averaging means **summing** $a$ for a **range of indices** and **dividing** by the **number of values**. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211016194331775.png)

- Each case $\to$ **normalization** constant is **chosen**
  - So if we **smooth** a **constant** function.
    - The **result** is the **same function**.
- Idea of a **moving average** $\to$ is *essence* of **convolution**
  - Only difference is **that in convolution** 
    - The **moving average** is a **weighted average**

## Discrete convolution

https://www.youtube.com/watch?v=KAOJsqCyd5Y - good example explanation

https://dsp.stackexchange.com/questions/5992/flipping-the-impulse-response-in-convolution - reason for formulae

- Convolve discrete sequence $a[i]$ with **another discrete sequence** $b[i]$ 
  - The **result** is a **discrete sequence** $(a\star{b})[i]$ 
    - Just like **smoothing** $a$ with a **moving average**
      - Rather than **equal weights** of every sample within **distance** $r$ 
        - use some second sequence $b$ to **weight each sample**

- $b[i-j]$ gives **weight** for sample at **position** $j$ 
  - This is at **distance** $i-j$ from the **index** $i$ where we **evaluate convolution**
    - Here is a **definition** of $a\star{b}$ 
      - Expressed in formulae:

https://www.youtube.com/watch?v=8mID2Dvg110

https://larzeitlin.github.io/CNV/ - visualisation and further explanation

- Essentially you take $j$ as the **sample** from **sequence** $a$ and as **j increments**
  - You scale up by **varying 'delays'** in the sequence of $b$ from **i** 
  - Adding up all of these values up **to j samples** calculates this **single value** $i$ to create the convolved sequence.
  - You **flip** the $b$ and increment $j$ along each time as **to move it across a**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211016195053627.png)

- We need to flip b because we want the **beginning of b** to meet the **beginning of a** first, and the **end of b** to meet the **end of a** last, as it does in the animation above. If we didn’t flip that wouldn’t happen.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211016195140462.png)

- *filter b missing from first line*

---

- Omission of **bounds** on $j$ 
  - Indicate this **sum** will **run over all integer**
    - That is from $-\infty$ to $+\infty$ 

- Illustrates **how one output sample** is **computed** 
  - Using the example $b=\frac{1}{16}[\dots, 0,1,4,6,1,0,\dots]$
    - This is $b[0] = \frac{6}{16}$ 
    - $a[\pm 1]= \frac{4}{16}$ for example.

---

- For **graphics**
  - One of the **two functions** often has **finite support**, means that its **non zero** over a **finite interval** of **argument values**
    - Assume that $b$ has **finite support** 
      - There is some radius $r$ such that $b[k] =0$ whenever $|k|>r$ 
        - In this case $\to$ write the **sum above** as:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211017192303170.png)

- Express this **definition** in code as:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211017200621780.png)

## Convolution filters

- Use convolution to **perform filtering**

  - Reinterpret that **smoothing operation** as **convolution** with a **particular sequence** 

- Computing **average** of **limited indices range**s

  - Same as **weighting** all points in **range** as **identical** and **weighting rest** of points with **zeros**.

- A **constant value filter** over some **interval** where its **non zero**

  - Called a **==box filter==** (*rectangular* if you draw some graph)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211017202303947.png)

- Box filter of **radius** $r$ $\to$ weight is $\large \frac{1}{2r+1}$  

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211017202253725.png)

- Substituting the **filter** to equation $9.2$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211017192303170.png)

  - Find it **reduces** to a **moving average** :

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211017202809481.png)

- conv filters often designed to **sum to 1**
  - Therefore do **not affect** the **overall level** of **the signal**

### Example - Convolution of box and a step

- Simple example of filtering
  - Let the *signal* be some **step function**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211017203004595.png)

- The **filter** is the **five point box filter** centered at **zero**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211017203038652.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211017203110672.png)

- Result of **convolving** $a$ and $b$ 
  - At some index $i$ $\to$ shown **above**
    - The **result** is the **average** of the **step function** over range $i-2$ to $i+2$ 
      - If $i < -2$ $\to$ average **zeros** therefore result is $0$.
      - if $i \geq 2$ averaging **all ones** therefore result **one**. 
    - Between there are $i+3$ ones
      - Results in **value** of $ \large \frac{i+3}{5}$ 
        - output = **linear ramp** that goes from $0$ to $1$ over **five samples**:
          - $\frac{1}{5}[\cdots, 0,0,1,2,3,4,5,5,\cdots]$ 

## Properties of convolution

- May seem **asymmetric**
  - $a$ is the **sequence** we **smooth** with $b$ providing **weights** 
    - Does **not make a difference** which one is the **filter** 
      - The **signal / filter** are **interchangeable** 

- Other properties:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211018184558141.png)

- Use to **shorten convolutions** that are **consecutive**, example $((a \star{b_1})\star b_2)\star b_3$
  - If some **sequence** is **long** and filters are **short** (*small radii*) 
    - Faster to **convolve** the **three** filters together then **apply** to $a$ 

---

- A **simple filter** *serves* as some **identity** for **discrete convolution** 
  - Discrete filter of **radius zero**
    - Or sequence $d[i] = \cdots, 0,0,1,0,0,\cdots$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211018185623135.png)

- If you **convolve** $d$ with signal $a$ 
  - There is **one nonzero term** in the **sum**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211018185707195.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211018185743594.png)

- Convolving $a$ with $d$ gives $a$ again

  - Sequence $d$ known as **discrete impulse**
    - Occasionally **useful** in **expressing** a **filter**
      - Process of **smoothing** some signal $a$ with filter $b$ 
        - Then **subtracting** from **original** can be **expressed** as **single convolution** with filter $d-b$ :

  

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211018190036218.png)

## Convolution as a sum of shifted filters

- Omit $[i]$ and consider the **entire sequence** of the **convolution**

  - If $b$ is some sequence, same sequence shifted to the **right** by $j$ is $\large b_{\to j}$

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211018190808771.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211018190620365.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211018190743604.png)

- Represents the **entire sequence** now.

---

- The convolution is therefore a **sum** of **shifted copies** of $b$

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211018190921433.png)

  - Each **weighted** by the entries of $a$ 
    - Via **commutativity** $\to$ can pick either $a$ or $b$ as the **filter**
      - Choosing $b$ $\to$ add up a **single copy** of the **filter** for **every sample** in the **input**.

## Convolution with continuous functions

https://eem.eskisehir.edu.tr/aaybar/EEM%20209/duyuru/convolution.pdf - good simple overview graphically.

https://www.youtube.com/watch?v=1pJ5ie2hX1M

https://www.youtube.com/watch?v=_L-GifnxJZQ

https://www.youtube.com/watch?v=zoRJZDiPGds - best explanation 

  - Sample sequences often supposed to **represent** some **continuous function**

---

- Convolution of **two continuous functions** is the **generalization** of the **discrete equation**
  - With some **integral replacing** the **sum**: *equation 9.3*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211018192037405.png)

- One way of **interpreting** the **definition** 
  - Is that the **convolution** of $f$ and $g$ 
    - Evaluated at **argument** $x$ 
      - Is the **area under the curve** of the **product** of **two functions**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211018192241679.png)

- After shifting $g$ so that $g(0)$ lines with $f(t)$ 
  - Just as **in discrete case**.
    - The convolution is a **moving average**, with the **filter providing weights** for the **average**

- Like **discrete convolution**
  - Convolution of **continuous functions** is **commutative** / **associative** and **distributive over addition**
- Like with **the discrete case**
  - The **continuous convolution** seen as a **sum of copies** of the filter rather than the **computation of weighted averages**
    - Except for this case $\to$ there are **infinitely many copies** of said filter $g$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211018192704018.png)

### Example - Convolution of two box functions

https://www.usebackpack.com/resources/5476/download?1440513913

- Let $f$ be some **box function**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211018192802941.png)

- What is $f \star f$ 

  - Definition **equation 9.3** obtains:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211018192837237.png)

- Figure below shows the **two cases** of this **integral**
  - The **two boxes** may have **zero overlap**
    - happens when $x \leq -1 $ or $x \geq 1 $ 
      - For this case $\to$ the **result** is **zero**
- When $-1 < x < 1$ 
  - The **overlap depends** on the **separation** between the **two boxes**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211018193108672.png)

- This creates $| x |$ 
  - The result is $1 - |x|$
    - Therefore 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211018193310631.png)

- The function $\to$ known as **tent function**
  - Another **common filter**
    - Basically as they overlap one another they build up an area at max of 1 when they are complete over one another then decrease back down to 0 again
    - this obviously will only occur between the ranges that they overlap in which is as defined: $-1 < x  < 1$ 
    - line drawn following t, when we translate and shift one sequence
      - t often starts at the origin to in this case it means its in the center with the edges being -1/2 and 1/2 respectively, when the edges touch non zero values appear on the output graph to show the convolution output.

![Convolution of box signal with itself2.gif](https://upload.wikimedia.org/wikipedia/commons/6/6a/Convolution_of_box_signal_with_itself2.gif)

## Dirac delta function

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211018193953486.png)

- For **discrete convolution**
  - Saw that *discrete* impulse $d$ acted as **an identity** 
    - $d \star a = a$ 
  - For the **continuous cases**
    - There is an **identity function** called the *dirac impulse* or *dirac delta* function denoted $\delta(x)$ 
  - The **delta function** is *narrow* and tall but **infinitesimal width** with area equal **to one**.
    - Key property is that **multiplying** it by a **function** selects the **value exactly** at **zero**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211018194532036.png)

- The **delta function** does *not* have some **well defined value** for $0$ 
  - Loosely think of it as $+\infty$ 
    - Does have a value $\delta (x) = 0 \ \ \forall \ x \neq 0$  

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211018194758955.png)

- Selecting out **single values** via this property

  - Follows that the **delta function** is the **identity** for **continuous convolution** as convolving $\delta$ with any function $f$ yields:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211018195242777.png)

- Builds **piecewise functions** and then plot this to **view** the **output response**
  - As in continuous there are **various phases** of the **convolution**

## Discrete Continuous convolution

- Two methods to **connect** *discrete* and *continuous worlds*
  - One via **sampling**
    - Convert a **continuous function** $\to$ **discrete** by **writing** the *functions value* at **all integer arguments** and **forgetting the rest**.
- Given some **continuous function** $f(x)$ 
  - *Sample* $\to$ **convert** to a **discrete sequence** $a[i]$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019000156399.png)

- The **other way** from some **discrete function** / **sequence**

  - To a **continuous function** $\to$ called a **reconstruction**

    - Accomplished using **another convolution form**

      - The **==discrete-continuous form==**.

- For this case $\to$ filter a **discrete sequence** $a[i]$ with **continuous filter** $f(x)$:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019000345493.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019001832781.png)

- The **values** of the **reconstructed** function $a \star f$ at $x$ is a **weighted** *sum* of the **samples** $a[i]$ for values of $i$ near $x$ 
  - Weights come from **filter** $f$,which is evaluated at **a set of points** spaced **one unit apart**
    - Example $x = 5.3$ and $f$ has **radius** $2$ 
    - $f$ is evaluated at $1.3, 0.3, -0.7$ and $-1.7$ 
- For **discrete continuous convolution**
  - Generally write **sequence** **first** and **filter second**
    - So the **sum** is over **integers**.

---

- Can put **bounds** on the **sum** if we **know** the **filter radius**  $r$ 
  - Therefore **eliminating** all points where the **difference** of $x$ and $i$ is at **least** $r$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019004316095.png)

- If some **point falls** at **distance** $r$ from $x$ (if $x-r$ is an **integer** for example)
  - its **left out**
    - This is in contrast to the **discrete cases** where you **include** the point at $i-r$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019004645475.png)

- Like **other convolution forms**
  - The **discrete continuous convolution** may be seen as **summing shifted copies** of the **filter**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019004823334.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019004833068.png)

- DCC is **related to splines**
  - For **uniform splines** (uniform B-spline, for instance)
    - The **parameterized curve** for the **spline** is the **exact convolution** of the **spline basis function** with **control point sequence**.

## Convolution in more than one dimension

- Sampling / reconstruction done in **one dimension**
  - Some singular variable $x$ or sequence *index* $i$ 
- Apply concepts to 2D, 3D, etc. 
  - Such as for **image processing** 

---

- Start with **definition** of a **discrete convolution**
  - Generalize to **2 dimensions** by forming some **double sum**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019011521886.png)

- If $b$ is **finitely supported filter** of radius $r$ ($(2r+1)^2$ values)
  - Write the sum **with bounds**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019011621480.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019011752604.png)

- Express in code:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019012018081.png)

- Definition **interpreted** in same sense for **1D**
  - Each **output sample** is some **weighted average** of an **area** in the **input**
- Uses the **2D filter** as a **mask** of **weight determination** for *each sample* in the **average**
- Continue the **generalization**
  - Write **continuous - continuous**  and **discrete continuous** convolutions in **2D also**.  

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019012348770.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019012408100.png)

- Each case

  - The **result** at some **point** is a **weighted average** of the **input** *near the point*

    - For **cont - cont** case $\to$ its a **weighted integral** over a **region centred** at that **point**

    - For **disc - cont** case $\to$ its a **weighted average** of all **samples** that fall **near the point**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019012535192.png)

- Once you go from 1D to 2D , it can be **generalized**. 

## Convolution filters

- Each filter has some **natural radius** for use in **graphics**

  - The **default size** to be used in **sampling / reconstruction**
    - When samples are **spaced** at **one unit apart** 

- For this section $\to$ **filters** are defined at this **natural size**

  - The **box filter** $\to$ has natural radius $\frac{1}{2}$ 
  - **Cubic filter** $\to$ natural radius $2$ 

- Arrange for **each filter** to **integrate** to $1$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019183317036.png)

- This is the **requirement** for **sampling / reconstruction** with no change to **signal average value**

---

- See later $\to$ some **applications** require **filters** of **different sizes**
  - Obtained by **scaling the basic filter** 
    - For some filter $f(x)$ $\to$ define some version of **scale** $s$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019183707212.png)

- Filter **stretched horizontally** by some factor $s$
  - Then **squashed vertically** by $\frac{1}{s}$ as to **keep the area unchanged**
    - Filter has natural radius $r$ and used at **scale** $s$ with a radius **of support** $sr$.

---

### Gallery of convolution filters

#### Box filter

- This is a **piecewise constant function** whose **integral** is **equal to one**
  - As a **discrete filter** $\to$ can be **written as**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019184043067.png)

- For **symetry** $\to$ include **both endpoints**
  - As a **continous filter**
    - Write 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019184211631.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019184238457.png)

- For this case $\to$ **exclude** one single **endpoint**
  - Makes box of radius $0.5$ usable as **reconstruction filter**
    - Because the **box filter** is **discontinuous**  that these **boundary** cases are **important**
      - So for some **particular filter** $\to$ must **pay attention** to **them**.
        - Just write $f_{\text{box}}$ for the **natural radius** of $r=\frac{1}{2}$ 

#### Tent filter

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019184718261.png)

- The **tent filter** / **linear filter**
  - Is a **continuous ,piecewise linear function**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019184751128.png)

- Natural radius is $1$ 
  - For **filters** $\to$ such as this **one**
    - There are at **least** $C^{0}$ (there are **no sudden jumps** basically as there are **with box**)  
      - No longer **seperate** the **definitions** of the **discrete / continous filters**
        - The **discrete filter** is just a **continuous filter** but **sampled** at **integers**. 

---

#### Gaussian filter

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019184947376.png)

- The **gaussian function**
  - Known as the **normal distribution also**:

https://www.youtube.com/watch?v=cTyPuZ9-JZ0 - derivatisation of normal distribution

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019185017199.png)

- Parameter $\sigma$ called the **standard deviation**
  - The **gaussian** makes a **good sampling filter** as its **smooth**
- Does **not have a natural radius**
  - useful **sampling filter** for **a range** of $\sigma$ 
    - The **Gaussian** does *not have a finite radius* of support
      - Although via **exponetial decay** its values **rapidly become** *small enough* to ignore.

- When **necessary** 
  - Can **trim** the **tails** from the **function** via setting **zero** out of some radius $r$ 
    - Called a **trim gaussian**
      - Therefore the **filters width** and **natural radius** can *vary* depending on the **application**
        - *Trimmed gaussian* scaled by $s$ is the **same** as some **unscaled** *trimmed gaussian* with **standard deviation** $s \sigma$ and radius $sr$ 
- Best method at handling this **in practice** is to let $\sigma$ and $r$ be **set as properties** of said **filter**
  - *Fixed* $\to$ when the **filter** is **specified**
    - Then **scale** the **filter** just like **any other** when its **applied**

#### B-Spline cubic filter

- Often filters defined as **piecewise polynomials** and **cubic filters** with **four pieces** (*natural radius of 2*) used as **reconstruction filters**
  - One such is the **B-spline filter** because of its **origins** as a **blending function** for **spline curves**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019190408716.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019190419434.png)

- Among **piecewise cubics**
  - The **B-spline** is special as it has **continous first** and **second derivatives** 
    - Its $C^{2}$ $\to$ more **concise** is defining this **filter** as :

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019190523243.png)

#### Catmull Rom Cubic filter

- Another **piecewise cubic filter** for a **spline**
  - Has its **value zero** at $x=-2,-1,1,2$ 
    - Therefore **interpolate samples** when used as **reconstruction filter**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019190713219.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019190736852.png)

#### Mitchell-Netravali cubic filter

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019190923255.png)

- For applications of **resampling images**
  - There is a study of **cubic filters** and **recommended** one **partway** between the **previous two filters** at the **best all around choice**
    - Its just a **weighted combination** of the **previous two filters**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019190912957.png)

### Properties of filters

- The **==impulse response==** of a **filter** is the name for the **function**
  - The **response** of the **filter** to some **signal** that just **contains an impulse**
    - (RECALL > CONVOLVING WITH AN IMPULSE OBTAINS THE FILTER)

---

- A **continuous filter** is **interpolating** 
  - If, when it is **used** to **reconstruct** a **continuous function** from a **discrete sequence**
    - The **resulting function** takes on some **exactly the values** of the **samples** at the **sample points**
      - It **connects the dots** rather than **producing some function** that **goes near the dots**

---

- Interpolating **filters** are **exactly these filters** $f$ for which $f({0}) = 1$ and $f(i)=0$ 
  -  For every **non zero integers** $i$ 
- A **filter** that takes on **negative values** holds **ringing** / **overshoot**
  - Produce **extra oscillations** in the **value** around **sharp changes** in the **value** of the **function being filtered**

---

- The **Catmull Rom filter**
  - Has **negative lobes** for **each side**
    - Adding a **filter step function**
      - This just **exaggerates** the **step** a **bit**
        - Results in **function values** that **undetshoots** 0 and **overshoots** 1
- A **continuous filter** is **ripple free**
  - if, when used as **reconstruction**
    - It **reconstructs** a **constant sequence** as a **constant function**
      - This *is equivalent* to the **requirement** that the **filter** sum to **one** on **any integer spaced grid**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019191613845.png)

- Every filter in this section are **ripple free** at **natural radii**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019191706229.png)

- Apart from the **gaussian**
  - None **of them are technically ripple free**
    - When used at a **non integer scale**
- Its **necessary** to **eliminate** ripple in **disc - cont  conv** 
  - Easy
    - **Divide** every computed sample by the **sum** of the **weight** used to **compute it**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211019191849890.png)

- interpret this **as a convolution** between $a$ and a filter $f$
  - Some **continuous filter** has a **degree of continuity**
    - This is the **highest order derivative** that is **defined everywhere**
- A **filter** , like the **box filter**
  - That has **sudden jumps** is **not continuous** at all
    - A **filter** that is **continuous**  but **with sharp corners** $\to$ *discontinuities* in the **first derivative**
      - Such as **tent filter** $\to$ has order **of continuity zero** $\to$ say its $C^{0}$ 

- Filter that is **cont** such as **piecewise cubic filters**  is $C^{1}$ 
  - If the **second derivative** is **cont** then its $C^2$ such as in **B spline filter**
- The **order of continuity** of some **filter** is **important** for the **reconstruction filter**
  - The **function** inherits this **continuity**.

### Separable filters

- Any **2D function** can be a **2D filter**
- Most useful method is a **separable filter**
  - The value at $f_2(x,y)$ at some $x$ and $y$ is the **product** of $f_1$ the **1D filter** Evaluated at $x$ and $y$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211021053147068.png)

- A **horizontal / vertical slice** throughout $f_2$ is a **scaled copy** of $f_1$ 
  - The **integral** of $f_2$ is the **square integral** of $f_1$
    - If $f_1$ is **normalized** then so is $f_2$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211021053302395.png)

#### Example - separable tent filter

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211021053330751.png)

- Profiles along **coordinate axes** are **tent functions**
- Profiles along **diagonals** are **quadratics**
  - Along $x=y$ in the **positive quadrant** $\to$ $(1-x)^2$

#### Example - 2D Gaussian filter

- Choosing the **gaussian filter**

## Signal processing for images

## Image filtering using discrete filters

- Blur $\to$ done via **low pass filters** such as **box > gaussian**

- Gaussian $\to$ creates **smooth effect** for blur

- Opposite of blur = sharpen $\to$ use **unsharp mask **https://homepages.inf.ed.ac.uk/rbf/HIPR2/unsharp.htm

  1. Subtract some fraction $\alpha$ of a **blurred image** from the **original image**

  2. Rescale to avoid **changing brightness / intensity**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211021053707579.png)

  3. Where $\large f_{g, \sigma}$ is the **gaussian filter** with width $\sigma$ 

  4. Uses discrete impulse $d$ and **distributive** convolution property, can write the **entire process** as **one filter** that depends on **width of blur** and **degree of sharpening** 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211021053944022.png)

---

- Further example of **combining** *two discrete filters* $\to$ drop shadow
  - Common to take **blurred** and **shifted copy** of some **objects outline** to create a **soft drop shadow**
    - Express **shifting operation** as **convolution** with **off center impulse**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211021172956827.png)



![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211021173005663.png)

- Shifting , blurring is achieved by **convolving** against both filters.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211021173033277.png)

Antialiasing in image sampling

- In **image synthesis** $\to$ must produce some **sampled representation** of an **image**
  - There is some **continuous mathematical formulae** that defines the image
    - (*or some procedure* that is used to **computer the color** at some point and not just **pixel positions**).
- *Ray tracing* $\to$ common example
  - In language of **signal processing** $\to$ have some cont **2D signal** (*image*)
    - That we **sample** on some **regular 2D lattice**
      - Going ahead to **sample** the **image** with **no special measures in place**
        - Result exhibits many ==**aliasing artificacts**== 
- For **sharp edges** $\to$ view a **stair step** sort of artefact known as **jaggies**
  - Areas with **repeating patterns**
    - See **wide bands** called **moiré patterns**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211021182152511.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211021182255958.png)

-  Image contains many **small scale features**
   - must **smooth out** by **filtering** before we **sample**
     - Definition of **cont conv** 
       - Must **average** the **image** over some **area** around **pixel location**, Rather than just taking the **value** at some **singular point**
-  Methods of this are discussed in chapter 4.
   - Simple filters such as **box** improve the **appearance** of **sharp edges**
     - Still produce **moiré patterns** 
-  Gaussian filter $\to$ very **smooth** $\to$ more **effective** against the **moire patterns**
   - At the **expense** of **overall somewhat blurring more prevalent** 
     - These 2 examples show tradeoff with **sharpeness** and **aliasing**
       - That is **fundamental ** to selecting **antialiasing filters**

## Reconstruction and Resampling

- Operation in images where **filtering** must **take care** is *==resampling==* 
  - Changes the **sample rate / image size**
- 3000 x 2000 camera > 1280 x 1024 monitor
  - To fit, must **maintain** the $3:2$ aspect ratio
    - must **resample** it to 1278 x 852 pixels.
      1. One method is **pixel dropping** 
         - Very **fast / good image preview** 
         - Quality becomes much **lower**
- Use **resampling** operation instead
  - We require some set **of samples** of the **image** on some **grid** that is **defined** by the **the new** image dimensions

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211021185309080.png)

- We obtain them via **sampling** the **continuous function** reconstructed from the **input samples**.
  - Sequence of standard **image processing operations**
    1. First **reconstruct** a **cont function** from the **input samples**
    2. Sample that function just as with **other continuous images**
    3. Avoid aliasing artifacts via using **appropriate filters** at **each stage**.
- Small example shown below

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211021185632949.png)

- OG = 12 x 9 pixels
- NEW = 8 x 6 pixels
- there are $\frac{2}{3}$ as many output as input in each dimension therefore the spacing across the image is $\frac{3}{2}$ spacing of **original**
- Value for every **output sample** $\to$ compute values for **image** which is **in-between the samples**
- Pixel dropping algorithm $\to$ gives us **one way** to do this
  - Take the **value** of the **closest** sample in the image with **1 pixel wise box filter** and then **point sampling**
    - If main reason for pixel dropping is **performance**, never implement that method as a special case of general reconstruction / resampling procedure.
- As of **discontinuities**
  - Hard to ensure **box filters** work
    - For **HQ sampling** $\to$ the **reconstruction / sampling framework** provides **valuable flexibility**.
- Algorithm
  - Drop to 1D and discuss the **resampling of some sequence**
    - Simplest method to write an **implementation** is via terms of **reconstruct function** defined in earlier section

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211021190145310.png)

- $x_0$ gives the pos of **first sample** of **new sequence** in terms of the old samples sequence
  - If first output sample falls **mid between** $3$ and $4$ in the **input sequence**
    - Then $x_0$  = 3.5
- This procedure **reconstructs** some **continuous image** via **convolving** the input sequence with some **cont filter** 
  - Then **points samples to it**
    - These dont happen sequentially, cont function only exists in **premise**. 
- Computes set of point of function $a \star f$ 
  - Combine reconstruct and **sampling filter in one**. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211021190908873.png)

- Issue is the **edges of the image**
  - Simple one here accesses **beyond bounds** of input
    - Several things you can do
      1. Stop loop at end of sequence, padding zeros 
      2. Clip all array accesses to end of the sequence
         - Return a[0] when accessing a[-1] 
         - pad edges with extension of last column
      3. Modify filter as **we approach edges** to not extend beyond bounds of the sequence.

- First = dim edges after resampling
- second = easy
- third = best performance
- Modify filter $\to$ **renormalizing**
  1. Divide filter by sum of the part of the filter that **falls within the image**
  2. Therefore filters adds to one over actual image samples thus preserving **intensity**
  3. Performance $\to$ handle band of pixels within a filter radius of the edge seperately from the center (contains **more pixels** and does **not require normalization**)
- Choice of filter in **resampling important**
  1. Shape 
  2. Size
- Reconstruction
  - smooth filter to avoid aliasing when enlarging
  - ripple free
- Sampling
  - filter should be large to avoid under sampling 
  - smoot to avoid moiré artefacts.
- Choose on filter shape / scale to relative resolutions of input / output.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211021191453622.png)

- The **lowest band** of resolution determines size
  - output = more coarse sampling than the input (**down sampling / shrinking**)
    - The smoothing required for the **proper sampling** is **greater** than the **smoothing required** for **construction** 
      - Therefore size the **filter according** to the **output sample spacing**
  - Output = fine sampling in **up sampling**. smooth required for reconstruction dominates, so size  of the filter determined by **input sample spacing** 

- Choosing a filter is a **tradeoff** in **speed quality**
  - Common = box
  - tent = moderate quality
  - piecewise cubic = really hq

- separable to **resample rows** then **width seperately.**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211021191834739.png)

https://www.cl.cam.ac.uk/techreports/UCAM-CL-TR-261.pdf - further reading

https://www.microimages.com/documentation/TechGuides/77resampling.pdf