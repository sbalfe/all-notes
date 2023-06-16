# Derivatives

- Each **weight / bias** $\to$​ may have a variety of **different degrees of influences** to the **loss**
  - Dependent on the **parameter themselves** / **current sample** $\to$ which inputs to the **first layer**

- Input > multiplied by **weights**
  - Therefore the **input data** affect the **neuron output**.
    - Therefore affect the **impact** the **weight** creates on the **loss**.
- Same idea applies to **biases** and **proceeding layers** 
  - Which of course take the **previous output layer** as **input**
    - The **input on output** $\to$ depends on **parameters** alongside **samples**

---

- The **function** of *how* some **bias / weight** impact **overall loss** is *not linear always*
  - To understand how to **adjust weights  / biases**
    - Understand their **impact** on the **loss**

---

> We refer to **weights and biases** and their impact on the **loss function**

- The **loss function** contains *no weights* or *biases*
  - The **input** is the **model output**
    - The **weights biases** *influence* this **output**
- So we calculate loss from output, **not weight / bias**
  - but the **weight bias** will **directly impact loss**

---

#### Topics Discovered From now on

1) Backpropagation
2) partial derivatives
3) gradients
4) gradient descent 

- Calculate *how much each singular weight* and *bias* has on **loss**
  - The overall **impact essentially** given some **sample**.
    - Each sample **produces** a **seperate output** and therefore a **seperate loss value**.
- Provides information on how to **alter bias / loss** to **lower the loss**

---

- GOAL  = **REDUCED LOSS** overall. 
  - Done via **==GRADIENT DESCENT==** 
- **==GRADIENT==** is the result of **==PARTIAL DERIVATVE==** calculations 
  - Then **==BACKPROPOGATE==** via **chain rule** to update the **weights and biases**

---

## Impact of a parameter on the output

- Simple $y=2x$ function to **investigate impact**

- Visualise:

```python
import matplotlib.pyplot as plt
import numpy as np
def f ( x ):
	return 2 * x

x = np.array( range ( 5 ))
y = f(x)
print (x)
print (y)
>>>
[ 0 1 2 3 4 ]
[ 0 2 4 6 8 ]
plt.plot(x, y)
plt.show()
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210812225142938.png)

## Slope

- y is **double x**
  - Alternatively describe via slope.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210812225311186.png)

- Calculate:
  1. Take any **two points** on the functions graph
  2. **Subtract** them to **calculate change**
     1. This means to subtract **x** and **y** dimensions respectively

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210812225455988.png)

- Keep each value of **x** in a **1D** **numpy array** $\to$ $x$ 
- Results in **1D numpy array** $\to$ $y$ 
- perform same operation:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210812225619314.png)

- Slope clearly 2.

> The **measure of impact** that *x* has on **y** is **==2==**

---

- Obtain same from any **linear graph**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210812225900261.png)

- Measuring from various points gives us **various slopes** that **increase**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210812230001527.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210812230018650.png)

- Slope at **tangent line** = **instantaneous slope**
  - This is the **derivative**.
    - Tangent created from **drawing line** at some **specific point**.
      - Between two **infinitely close points technically.**

- Must be **continuous and smooth** of which we can **not calculate** slope from
  - Has a **sharp corner**.
- Can **approximate derivatives** at $x$ by taking this point along with $x+\delta$ where $\delta$​ is a **very small number** such as $0.0001$ 
  - Common for this number as it has **small error**.
    - Prevent **instability numerically** $(\Delta{x})$ could round to **0** via **floating point resolution**.
      - Perform same calculation but on **points very close**.
        - Results in **extremely good approximation** of **slope at x**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210812230748379.png)

- The true value is **4** taking $\large(\frac{dy}{dx}(2x^2))$​​​  
- Cannot surpass python limitation on **floating point precision**
  - Solution is **restricted** between **estimating derivative** and **remaining numerically stable**
    - Introduces **small but visible error**.

## Numerical Derivative

- Above was intro to **numerical differentiation**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210812231356042.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210812231432815.png)

- The **closer** the **points**
  - The **more correct** the **tangent becomes**
    - Visualise the **tangent lines** and how they **change** depending on how **we calculate them**.

```python
import matplotlib.pyplot as plt
import numpy as np
def f ( x ):
	return 2 * x ** 2
# np.arange(start, stop, step) to give us smoother line
x = np.arange( 0 , 5 , 0.001 )
y = f(x)

plt.plot(x, y)
plt.show()
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210812231600408.png)

---

- To **draw** the **tangent lines**
  - Derive function for **tangent line** at some point then **plot** on **graph**
  - $m$​ is the **approximate derivative**, already calculated.
  - Leaves for us **to calculate b** 
  - Can move the **line up and down** using this like the **bias**.
    - $y=mx+b$​ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210812231701939.png)

- **y and x** are **decided**
  - We can vary the output of **b** however.

- Calculate **b**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210812231849006.png)

- So far $\to$ use **two points**
  - That we want the **derivative** that are **close enough** to it **to calculate**
    - Given above equation $\to$ approximation of the **derivative** and the same **close enough points** (x , y to be specific)
- Substitute these in the **equation** to obtain **y intercept** for the **tangent line** at  the **derivation point**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210812232046388.png)

```python
import matplotlib.pyplot as plt
import numpy as np
def f ( x ):
	return 2 * x ** 2

# np.arange(start, stop, step) to give us smoother line
x = np.arange( 0 , 5 , 0.001 )
y = f(x)
plt.plot(x, y)

# The point and the "close enough" point
p2_delta = 0.0001
x1 = 2
x2 = x1 + p2_delta
y1 = f(x1)
y2 = f(x2)
print ((x1, y1), (x2, y2))

# Derivative approximation and y-intercept for the tangent line
approximate_derivative = (y2 - y1) / (x2 - x1)
b = y2 - approximate_derivative * x2


# We put the tangent line calculation into a function so we can call
# it multiple times for different values of x
# approximate_derivative and b are constant for given function
# thus calculated once above this function
def tangent_line ( x ):
	return approximate_derivative * x + b

# plotting the tangent line
# +/- 0.9 to draw the tangent line on our graph
# then we calculate the y for given x using the tangent line function
# Matplotlib will draw a line for us through these points
to_plot = [x1 - 0.9 , x1, x1 + 0.9 ]
plt.plot(to_plot, [tangent_line(i) for i in to_plot])


print ( 'Approximate derivative for f(x)' ,
			f 'where x = {x1} is {approximate_derivative} ' )
plt.show()
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210812232353051.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210812232403042.png)

- This is the **estimated tangent** at $x=2$ 
  - We care for the tangent as its the **point** of **instantaneous rate of change**
    - Use this idea to **determine effect** of a *specific weight / bias* on the **overall loss** of a function in some **sample**
- With **different values** of $x$ 
  - Observer the **resulting impacts** on the **function**
    - Continue previous code to view **tangent lines** of **various inputs $x$** 
- Place a part of the code in a **loop** over **example** $x$ values and **plot multiple tangent lines**

```python
import matplotlib.pyplot as plt
import numpy as np
def f ( x ):
	return 2 * x ** 2

# np.arange(start, stop, step) to give us a smoother curve
x = np.array(np.arange( 0 , 5 , 0.001 ))
y = f(x)
plt.plot(x, y)
colors = [ 'k' , 'g' , 'r' , 'b' , 'c' ]

def approximate_tangent_line ( x , approximate_derivative ):
	return (approximate_derivative * x) + b

for i in range ( 5 ):
    p2_delta = 0.0001
    
    x1 = i
    x2 = x1 + p2_delta
    y1 = f(x1)
    y2 = f(x2)
    print ((x1, y1), (x2, y2))
    
    approximate_derivative = (y2 - y1) / (x2 - x1)
    
    b = y2 - (approximate_derivative * x2)
    
    to_plot = [x1 - 0.9 , x1, x1 + 0.9 ]
    
    plt.scatter(x1, y1, c = colors[i])
    
    plt.plot(
        [point for point in to_plot],		    [approximate_tangent_line(point,approximate_derivative)
    for point in to_plot],
    c = colors[i])
    print ( 'Approximate derivative for f(x)' ,
    f 'where x = {x1} is {approximate_derivative} ' )
plt.show()
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210812234655417.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210812234705103.png)

---

- This is not much of a **penalty**

---

- Our **neural network** is not as simple
  - Loss function contains every **layer / weight / bias** 
    - A very **large function** in **various dimensionalities** 


----

- Calculate derivatives using **numerical differentiation**

  - Require **multiple forward** passes for a **single parameter update** (C7)
  - We basically change a parameter > forward > pass > use delta loss to update
  - But for every single variable, as explained further below


---

- Perform **forward pass** as a *reference*

  - Update a **single parameter** via some **delta value**
    - Then **perform** the **forward pass** through the model again to **view loss**


---

- Calculate **derivative** now and **revert the parameter** change we **made** for *this calculation*.

  - **Repeat** for **every weight / bias** and for **every sample**
    - This is **very time consuming**
      - This is just **brute force** of the **derivative calculations**

  > **DERIVATIVE = SLOPE OF TANGENT LINE** for some **function** that takes **single parameter** as **input**

  > Use this idea to **calculate** the **slopes** of each **==Loss Function==** at each **weight / bias** points

- This brings to idea of **multivariate function** (weight and bias are considered.)
  - Take **multiple parameters** $\to$​ the **==PARTIAL DERIVATIVE==** 

---

## Derivatives Summary

- The **Derivative** of a *constant* **is 0**
  - This is where **m** is **constant** as we are **not deriving** with *respect to it*
    - This is **x**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210813000212590.png)

- Derivative of $x$ is $1$​ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210813000350759.png)

- Derivative of a **linear function** is its **slope**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210813000601388.png)

- Derivative of a **constant multiple** of the **function** equals the **constant multiple** of the **functions derivative**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210813000649325.png)

- Derivative of a **sum** of a **function** equals the **sum** of the **derivatives**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210813000735572.png)

- Same via subtraction

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210813001131749.png)

- Derivative of an **exponentiation** 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210813001230108.png)

- Use $x$ rather than **f(x)** has derivative of an **entire function** calculated slightly different
  - Explained along with **chain rule**

