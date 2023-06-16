# Backpropagation

- Understand how **to measure** the **impact** of a **variable** $\to$ **function output**
- Write **code** to **calculate the partial derivatives** to see how the **minimize** the **models loss**.

---

- Do **one neuron** for now.
  - BP from the **ReLU** function of a **single neuron** and intend to **minimize** the **output** for **this single neuron**.

---

- Overall idea in the end is **to minimize** our **loss function** of course.
- Start with **minimizing** the **more basic output** before **jumping** to **full network** and **overall loss**.

---

- **Recall** the **forward pass** and **atomic operations** for a **single neuron** and **ReLU activation**. 
  - Example with **3 inputs** to the **neuron**

```python
x = [ 1.0 , - 2.0 , 3.0 ] # input values
w = [ - 3.0 , - 1.0 , 2.0 ] # weights
b = 1.0 # bias
```

- Start with **input 1** `x[0]` , with **weight**, `w[0]`

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210915130911407.png)

- Multiply the **input** the **weight**

```python
x = [ 1.0 , - 2.0 , 3.0 ] # input values
w = [ - 3.0 , - 1.0 , 2.0 ] # weights
b = 1.0 # bias
xw0 = x[ 0 ] * w[ 0 ]
print (xw0)
>>>
- 3.0
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210915131356573.png)

- Repeat for `x1, w1 / x2, w2` pairs.

```python
xw1 = x[ 1 ] * w[ 1 ]
xw2 = x[ 2 ] * w[ 2 ]
print (xw1, xw2)
>>>
2.0 6.0
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210915131721570.png)

```python
# Forward pass
x = [ 1.0 , - 2.0 , 3.0 ] # input values
w = [ - 3.0 , - 1.0 , 2.0 ] # weights
b = 1.0 # bias
# Multiplying inputs by weights
xw0 = x[ 0 ] * w[ 0 ]
xw1 = x[ 1 ] * w[ 1 ]
xw2 = x[ 2 ] * w[ 2 ]
print (xw0, xw1, xw2)
>>>
- 3.0 2.0 6.0
```

- Next operation is **sum** all **weighted inputs** with some **bias**.

```python
Forward pass
x = [ 1.0 , - 2.0 , 3.0 ] # input values
w = [ - 3.0 , - 1.0 , 2.0 ] # weights
b = 1.0 # bias
# Multiplying inputs by weights
xw0 = x[ 0 ] * w[ 0 ]
xw1 = x[ 1 ] * w[ 1 ]
xw2 = x[ 2 ] * w[ 2 ]
print (xw0, xw1, xw2, b)
# Adding weighted inputs and a bias
z = xw0 + xw1 + xw2 + b
print (z)
>>>
- 3.0 2.0 6.0 1.0
6.0
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210915151717578.png)

- Forms the **neurons output**
  - Last step is applying **ReLU** *activation* function on this output.

```python
# Forward pass
x = [ 1.0 , - 2.0 , 3.0 ] # input values
w = [ - 3.0 , - 1.0 , 2.0 ] # weights
b = 1.0 # bias

# Multiplying inputs by weights
xw0 = x[ 0 ] * w[ 0 ]
xw1 = x[ 1 ] * w[ 1 ]
xw2 = x[ 2 ] * w[ 2 ]
print (xw0, xw1, xw2, b)

# Adding weighted inputs and a bias
z = xw0 + xw1 + xw2 + b
print (z)

# ReLU activation function
y = max (z, 0 )
print (y)

>>>
- 3.0 2.0 6.0 1.0
6.0
6.0
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210915200305693.png)

- Full **forward pass** of a **single neuron** and **ReLU** *activation function*
-  Treat **chained functions** as a **single large function** 
  - Takes input values `(x)` , weights `(w)` and bias `(b)`.
  - as **inputs** and **outputs** `y`. 
- The **large function** is made of **many smaller functions**
  - Multiplication of **inputs values** and **weights**.
    - Sum of **these** and **bias added**
      - Alongside a **max** function as the **ReLU** activation.
- A total of **3 chained functions**

---

- First step $\to$​​ **backproagate** the **gradients** by **calculating derivatives** / **partial derivatives**.
  - Use **chain rule**
    - The **chain rule** of a **function** *stipulates* $\to$ the derivative **for nested functions** like `f(g(x))` 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210915230538009.png)

- The **large function** mentioned , loosely **in context** of **our neural network**
  - interpreted as:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210916123333129.png)

- Or **matching the code**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210916123425013.png)

- Task $\to$ calculate **how much** each of the **inputs / weights / bias** $\to$ impacts the **output**.
  - Consider **what we need** to **calculate** for **partial derivative** of $w_0$ , as example

---

- Rewrite **equation** to some form for **derivative calculations**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210916124235888.png)

- There are **3 nested functions**
  1. ReLU
  2. Sum of **weighted inputs** and **bias**
  3. **Multiplication** of **inputs / weights**
- Calculate **impact** of the **example weight** $w_0$ 
  - On **output** $\to$ **chain rule** tells us to **calculate** derivative of **ReLU** in *respect* to **its parameter** $\to$ **sum**.
    - Then **multiply** by **partial derivative** of **sum operation** with respect to $\text{mul}(x_0,w_0)$ , as the **input** contains this **parameter** in question.
      - Then **multiply** with the **partial derivative** of the **multiplication** operation with respect to the $x_0$ input.
        - Simplified in **this equation**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210916125518478.png)

- We did **not include** the `ReLU` parameter.
  - which is the **full summation** with **sum parameters**
    - Are all **of the multiplications** of **inputs / weights**.
      - makes it longer **and harder to read**.
- Shows we **must calculate** the **derivatives** and **partial derivatives** of *every atomic operation* and **multiply** to **acquire** the **impact** that $x_0$ makes **on the output**.
  - Then **repeat** to **calculate** all of the **remaining impacts**.
    - The **derivatives** with **respect** to **weights / bias** informs us the **overall impact** and use this to **update** the **weights / bias**.
      - The **derivatives** with **respect** to **inputs** are used to **chain more layers** by *passing them* to the **previous** function in the **chain**.

---

- Multiply **chained layers** of **neurons** 
  - Then the **loss function**.
    - Must know **impact** of **given weight** or **bias** on the **loss**.
- Thus **calculate derivative** of the **loss function** 
  - Then **apply chain rule** with the **derivatives** of **all activation functions** / *neurons* in all **of the consecutive layers**.
- The **derivative** *wrt* the **layers inputs**
  - As **opposed** to the **derivative** with **respects** to **weights / biases**
    - Is **not used to update any parameters**
      - Used rather to **chain** to **another layer**
        - Thus we ==backpropagate== to the **previous layer** in the **chain**.

---

- **Backward pass** $\to$ calculate the **derivative of the loss function**
  - Use this to **multiply** the **derivative** of **activation function** of **output layer**. 
    - Use this to **multiply** by the **derivative** of the **output layer**
      - So on..... via each **hidden layer / activation functions**.
- Within the layers
  - The **derivative** *wrt* the **weights / biases** forms the **gradient** to **update** them.
  - The **derivatives** *wrt* **inputs** forms the **gradient** to **chain** with the **previous layer**.
  - This **layer** can calculate the **impact** of its **weights / biases** on the **loss** then **backpropagate** gradients on the **inputs further**.

---

- Example $\to$ assume **our neuron** receives **a gradient** of `1`  from the **next layer**
  - Made up value, this will **not affect** the **other values**
    - Easily **show all of the processes** 
      - Use the **color of red** for **derivatives**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210916133520880.png)

- Recall that **the derivative** of `ReLU()`  with **respect** to its **input** is `1` 
  - If the **input** is **greater** than *0* , and 0 otherwise.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210916133834223.png)

```python
drelu_dz = ( 1. if z > 0 else 0. )
```

- `drelu_dz` is the **derivative** of the **ReLU** function *wrt* to `z` 
  - use `z` rather than `x` from the **equation**.
    - As the **equation** denotes the **max function** in **general**
      - We are **applying** to the **neurons output.** $\to$ `z`. 

---

- The **input value** to `ReLU` function is `6` therefore the derivative **equals** `1`. 
  - Must use **chain rule** then **multiply** this **derivative** with the **derivative** *received* from the **next layer** which is `1` for the **purpose of this example**.

```python
# Forward pass
x = [ 1.0 , - 2.0 , 3.0 ] # input values
w = [ - 3.0 , - 1.0 , 2.0 ] # weights
b = 1.0 # bias

# Multiplying inputs by weights
xw0 = x[ 0 ] * w[ 0 ]
xw1 = x[ 1 ] * w[ 1 ]
xw2 = x[ 2 ] * w[ 2 ]

# Adding weighted inputs and a bias
z = xw0 + xw1 + xw2 + b

# ReLU activation function
y = max (z, 0 )

# Backward pass
# The derivative from the next layer
dvalue = 1.0

# Derivative of ReLU and the chain rule, ignore 1 as 
# if z is gonna make derivative 1 either way as its just a constant value.
drelu_dz = dvalue * ( 1. if z > 0 else 0. )
print (drelu_dz)
>>>
1.0
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210916143004044.png)

- Result with **derivative** of `1`.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210916143408211.png)

- Moves **backwards** throughout the **neural network** 
  - What is **the function** that comes **immediately** before we **perform** the **activation function**?

---

- This is the **sum** of **weighted inputs** with **bias**
  - Therefore calculate the **PD** of the **sum function** with **chain rule** 
    - Then **multiply** this with **partial derivative** of the **subsequent outer function** $\to$ *ReLU* 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210916145752748.png)

- Partial **derivative** of the **sum operation** is always `1` 
  - Regardless of the **inputs**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210916145955154.png)

- The **weighted inputs** and **bias** are **summed** at **this stage** 
  - Calculate the **partial derivatives** of the **sum operation** *wrt* **each of these**.
    -  **Multiplied** by the **PD** for the **subsequent function** via **chain rule**
      - Which is the `ReLU` function $\to$ `derelu_dz`. 

---

- The **first partial derivative**

```python
dsum_dxw0 = 1
drelu_dxw0 = drelu_dz * dsum_dxw0
```

- `dsum_dxw0` means the **partial derivative** of the **sum** *wrt* to the `x` input.
  - **Weighted** for the **0th** pair of **inputs / weights**
- `1` is the **value** of **this partial derivative** 
  - Multiply via **chain rule** with **derivative** of the **subsequent** function
    - The `ReLU` function.

---

- Apply **chain rule** and **multiply** the **derivative** of the `ReLU` function with **partial derivative** of the **sum** *wrt* to **first weighted input**.

```python
# Forward pass
x = [ 1.0 , - 2.0 , 3.0 ] # input values
w = [ - 3.0 , - 1.0 , 2.0 ] # weights
b = 1.0 # bias

# Multiplying inputs by weights
xw0 = x[ 0 ] * w[ 0 ]
xw1 = x[ 1 ] * w[ 1 ]
xw2 = x[ 2 ] * w[ 2 ]

# Adding weighted inputs and a bias
z = xw0 + xw1 + xw2 + b

# ReLU activation function
y = max (z, 0 )

# Backward pass
# The derivative from the next layer
dvalue = 1.0

# Derivative of ReLU and the chain rule
drelu_dz = dvalue * ( 1. if z > 0 else 0. )
print (drelu_dz)

# Partial derivatives of the multiplication, the chain rule
dsum_dxw0 = 1
drelu_dxw0 = drelu_dz * dsum_dxw0
print (drelu_dxw0)

>>>
1.0
1.0
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210916162347116.png)

- This **results** in a **partial derivative** of `1` again.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210916162536524.png)

- Perform **same operation** on the **next weighted input**

```python
dsum_dxw1 = 1
drelu_dxw1 = drelu_dz * dsum_dxw1
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210916163957341.png)

- Results with the **next calculate partial derivative**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210916164257440.png)

- Then the **final weighted input**

```python
dsum_dxw2 = 1
drelu_dxw2 = drelu_dz * dsum_dxw2
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210916164726102.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210916164738690.png)

- Then the **bias**

```python
dsum_db = 1
drelu_db = drelu_dz * dsum_db
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210916165220818.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210916171008404.png)

- Add the **partial derivatives** with the **applied chain rule** to *our code*

```python
# Forward pass
x = [ 1.0 , - 2.0 , 3.0 ] # input values
w = [ - 3.0 , - 1.0 , 2.0 ] # weights
b = 1.0 # bias

# Multiplying inputs by weights
xw0 = x[ 0 ] * w[ 0 ]
xw1 = x[ 1 ] * w[ 1 ]
xw2 = x[ 2 ] * w[ 2 ]

# Adding weighted inputs and a bias
z = xw0 + xw1 + xw2 + b

# ReLU activation function
y = max (z, 0 )

# Backward pass
# The derivative from the next layer
dvalue = 1.0

# Derivative of ReLU and the chain rule
drelu_dz = dvalue * ( 1. if z > 0 else 0. )
print (drelu_dz)

# Partial derivatives of the multiplication, the chain rule
dsum_dxw0 = 1
dsum_dxw1 = 1
dsum_dxw2 = 1
dsum_db = 1
drelu_dxw0 = drelu_dz * dsum_dxw0
drelu_dxw1 = drelu_dz * dsum_dxw1
drelu_dxw2 = drelu_dz * dsum_dxw2
drelu_db = drelu_dz * dsum_db
print (drelu_dxw0, drelu_dxw1, drelu_dxw2, drelu_db)
>>>
1.0
1.0 1.0 1.0 1.0
```

- Continuing **backwards** 
  - The **function** that **comes** *before* the **sum** is the *multiplication* of **weights / inputs**
    - The **derivative** for a **product** is whatever the **input** is **being** *multiplied by*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210916173002558.png)

-  The **partial derivative** of `f` with respect to `x` **equals** `y` 
- The **partial derivative** of `f` with respect to `y` **equals** `x` 
- Follow this rule $\to$ the **PD** of the **first weighted input** *wrt* to to the **inputs** `=` **weight** (*other input of the function*) 
- Then **apply chain rule** and **multiply** this **PD** with **PD** of **subsequent function**
  - The **Sum** which was **calculated earlier**

```python
dmul_dx0 = w[ 0 ]
drelu_dx0 = drelu_dxw0 * dmul_dx0
```

- Thus **calculating** the **PD** *wrt* $x_0$ input
  - The **value** is $w_0$ 
    - We apply the **chain rule** with the **derivative** of the **subsequent function**
      - Which is the `drelu_dxw0`.

---

- As we **apply the chain rule**
  - Working **backwards** taking the `ReLU` derivative.
    - Taking the **summing operations** *derivative*. 
      - Multiplying **both** and **so on**
        - This is called **backpropagation** using the **chain rule**.
- The **resulting output function gradient** is **passed backwards** through the **neural network** 
  - Uses **multiplication** of the **gradient** of **subsequent functions** from **later layers** with the **current one** 
    - Add the **partial derivative** code and **show it on the chart**.

```python 
# Forward pass
x = [ 1.0 , - 2.0 , 3.0 ] # input values
w = [ - 3.0 , - 1.0 , 2.0 ] # weights
b = 1.0 # bias

# Multiplying inputs by weights
xw0 = x[ 0 ] * w[ 0 ]
xw1 = x[ 1 ] * w[ 1 ]
xw2 = x[ 2 ] * w[ 2 ]

# Adding weighted inputs and a bias
z = xw0 + xw1 + xw2 + b

# ReLU activation function
y = max (z, 0 )

# Backward pass
# The derivative from the next layer
dvalue = 1.0

# Derivative of ReLU and the chain rule
drelu_dz = dvalue * ( 1. if z > 0 else 0. )
print (drelu_dz)

# Partial derivatives of the multiplication, the chain rule
dsum_dxw0 = 1
dsum_dxw1 = 1
dsum_dxw2 = 1
dsum_db = 1
drelu_dxw0 = drelu_dz * dsum_dxw0
drelu_dxw1 = drelu_dz * dsum_dxw1
drelu_dxw2 = drelu_dz * dsum_dxw2
drelu_db = drelu_dz * dsum_db
print (drelu_dxw0, drelu_dxw1, drelu_dxw2, drelu_db)

# Partial derivatives of the multiplication, the chain rule
dmul_dx0 = w[ 0 ]
drelu_dx0 = drelu_dxw0 * dmul_dx0
print (drelu_dx0)

>>>
1.0
1.0 1.0 1.0 1.0
- 3.0
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210916203235772.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210916203247320.png)

- Perform **same operation** for **other inputs** and **weights**.

```python
# Forward pass
x = [ 1.0 , - 2.0 , 3.0 ] # input values
w = [ - 3.0 , - 1.0 , 2.0 ] # weights
b = 1.0 # bias

# Multiplying inputs by weights
xw0 = x[ 0 ] * w[ 0 ]
xw1 = x[ 1 ] * w[ 1 ]
xw2 = x[ 2 ] * w[ 2 ]

# Adding weighted inputs and a bias
z = xw0 + xw1 + xw2 + b

# ReLU activation function
y = max (z, 0 )

# Backward pass
# The derivative from the next layer
dvalue = 1.0

# Derivative of ReLU and the chain rule
drelu_dz = dvalue * ( 1. if z > 0 else 0. )
print (drelu_dz)

# Partial derivatives of the multiplication, the chain rule
dsum_dxw0 = 1
dsum_dxw1 = 1
dsum_dxw2 = 1
dsum_db = 1
drelu_dxw0 = drelu_dz * dsum_dxw0
drelu_dxw1 = drelu_dz * dsum_dxw1
drelu_dxw2 = drelu_dz * dsum_dxw2
drelu_db = drelu_dz * dsum_db
print (drelu_dxw0, drelu_dxw1, drelu_dxw2, drelu_db)

# Partial derivatives of the multiplication, the chain rule
dmul_dx0 = w[ 0 ]
dmul_dx1 = w[ 1 ]
dmul_dx2 = w[ 2 ]
dmul_dw0 = x[ 0 ]
dmul_dw1 = x[ 1 ]
dmul_dw2 = x[ 2 ]
drelu_dx0 = drelu_dxw0 * dmul_dx0
drelu_dw0 = drelu_dxw0 * dmul_dw0
drelu_dx1 = drelu_dxw1 * dmul_dx1
drelu_dw1 = drelu_dxw1 * dmul_dw1
drelu_dx2 = drelu_dxw2 * dmul_dx2
drelu_dw2 = drelu_dxw2 * dmul_dw2
print (drelu_dx0, drelu_dw0, drelu_dx1, drelu_dw1, drelu_dx2, drelu_dw2)

>>>
1.0
1.0 1.0 1.0 1.0
- 3.0 1.0 - 1.0 - 2.0 2.0 3.0
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210916204054993.png)

- This is the **complete set** of the **activated neurons** *partial derivatives* *wrt* to the **inputs / weights / bias**
  - Recall the **equation** from the **start**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210916205025174.png)

##### Quick Summary of all above

> 1. So we performed backpropagation via chain rule starting from the ReLU of a singular function
> 2. This involves passing the derivative down to backpropogate , this passing down is just the step to calculate the overall partial derivative
> 3. The number of partial derivatives to compute ripples backwards for every weight/bias and input
> 4. weights and biases partial derivative vlaues are used to tweak them 
> 5. The inputs derivatives are passed down wards, thus backpropogate to further calculate weights and biases down and inputs down the line 
> 6. All links to the overall loss function cost as see later.

- Have the **complete code** and we are **applying chain rule** from **this equation**
  - See how to **optimize the calculations**.

---

##### Summary explanation of derivative

- So we wanted to minimize ReLU
- Therefore we apply the chain rule 
- Example is the input where the chain rule goes on to the weighted derivative wrt to r elu
  - Then goes down to the multiplication *wrt* to the input which is simply just the associated weight.

##### Derivatives of inputs

- Applied the **chain rule** to **calculate** the **PD** of the **ReLU** *activation function* *wrt* $\to$ to **the first input** $x_0$ 
  - Take **related** lines of code then **simplify them**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210917234531220.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210917234541239.png)

- Essentially not complex, just rewriting code for **easier reading**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210918002005258.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210918002020473.png)

- From **LHS** $\to$ the **derivative** calculated in the **next layer** *wrt* the **inputs**.
  - The **backpropagated** *gradient* to the **current layer** 
    - This is the **derivative** of the **ReLU** function.
      - And the **PD** of the **neurons function** *wrt* to $x_0$ input
        - All **multiplied** via applying **chain rule** to **calculate** the **impact** to the **neuron** on the **whole functions output**.

---

- The **PD** of some **neurons function** *wrt* to the **weight**
  - is the **input** *related* to **this weight** 
    - And *wrt* to the **input** is the **related weight**.
- The **PD** of the **neurons** function *wrt* the **bias** is **always** 1
  - We **multiply them** with the **derivative** of the **subsequent function** (`1` in this case)
    - To **obtain final derivatives**.

---

- Code all these derivatives in the **dense layers class** and the **ReLU** activation class for **backpropagation step**.
- Together the **partial derivatives** combined to some **vector** which form our **gradients**
- Represented as such:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210918003557033.png)

- For **this single neuron**
  - We do **not require** the `dx` 
    - With **many layers** $\to$ continue **backpropagating** to the **preceding layers** with the **PD** *wrt* to **our inputs**.

---

- Apply these gradients $\to$ **weights** to attempt to **minimize** the **output**
  - This is the **purpose** of the **optimizer** often
    - Show a **simplified** version of **the task** by directly Applying a **negative fraction** of the **gradient** to **our weights**.
- Apply a **negative fraction** to the gradient as we wish **to decreases the final output**
  - The **gradient** shows the **direction** of the **steepest ascent**
    - Example $\to$ our **current weights** and **bias are**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210918005001016.png)

- Apply a **fraction** of the **gradients** to **these values**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210918005030320.png)

- We **slightly** change the **weights / bias** in a way as to **decrease** the **output** 
- View the **effects** of the **tweaks** on the **output** by **doing** another **forward pass**

```python
# Multiplying inputs by weights
xw0 = x[0] * w[0]
xw1 = x[1] * w[1]
xw2 = x[2] * w[2]

# Adding
z = xw0 + xw1 + xw2 + b

# ReLU activation function
y = max(z, 0)
print(y)

>>>
5.985
```

- *Decrease* **output** from **6** to **5.895** 
  - This **decrease** is **non sensical** in a **real neural network** 
    - We actually wish to **decrease** the **loss value** 
      - The **final calculation** in the **chain** during the **forward pass**
        - And thus the **first calculation** for the **gradient** using **backpropagation**.

---

- We have **minimized** the **ReLU** of a **single neuron** to demonstrate the **overarching idea**

---

- Apply **one neuron** to **list of 3 samples** for **input**
  - Where **each sample** consists of **4 features** 
    - For this example $\to$ consists of **single hidden layer** that **contains** *3 neurons* (3 weights / 3 biases)
      - *Backward pass* explained again, but not forward.

---

- Backward pass with **single neuron so far**
  - Receive a **single derivative** to **apply chain rule**
    - Considering **multiple neurons** $\to$ in the **following layer**.
- A **single neuron** of **current layer** *connects* to **all of them**.
  - They **all receive** the **output** of **this neuron** 
    - What **happen** during **backpropagation** 
      - Each **neuron** from the **next layer** returns a **PD** of its function *wrt* to **this input**.
- The **neuron** in the **current layer** receives a **vector** of **these derivatives**
  - Must be some **singular value** for a **singular neuron**
    - To continue the **backpropagation** we must **sum this vector**

---

- Replace **current singular neuron** with a **layer of neurons**
  - Opposed to the **single neuron** $\to$ a **layer outputs** a *vector* of **values** instead of a **singular value** 
    - Each **neuron** in a **layer** *connects* to **all of the neurons** in the **next layer** 
- During **backpropagation** 
  - Each **neuron** from the **current layer** receives a **vector** of **partial derivatives** the same as **described** for a **single neuron** 
    - With a **layer of neurons** $\to$ takes form of a **list of these vectors** a **2D array**. 
- Must **perform a sum**
  - Each **neuron** must **output a gradient** of the **PD** *wrt* to all **inputs**
    - And **all neurons** will form a **list** of these **vectors**
      - We must **sum along the inputs**
        - The **first input** to **all the neurons**, **second** ...
- Sum the **columns**
  - To **calculate** the **partial derivatives** *wrt* to the **inputs**
    - We **require weights**
      - The **PD** *wrt* to the **input** is equivalent to the **related weight**.
- Therefore the **array** of **PD's** *wrt* to **every input** equals the **array of weights**
  - As this **array** is **transposed** $\to$ must **sum** the **rows** rather than **the columns**
- To apply the **chain rule** $\to$ must **multiply** them **by the gradient** from the **subsequent function**.

---

- Code below:
  1. Take in **transposed weights** 
     - The **transposed** array of the **derivatives** *wrt* to **the inputs**.
       - Then **multiply** by the **respective gradients** (*related to given neurons*).
         - To then **apply chain rule**
  2. **Sum** along with the **inputs**
  3. Calculate the **gradient** for the **next layer** in **backpropagation** 
     - The *next layer* in **backpropagation** is the **previous layer** in *the order* of **creation** of the *model*.

```python
import numpy as np

# Passed-in gradient from the next layer
# for the purpose of this example we're going to use
# a vector of 1s
dvalues = np.array([[1., 1., 1.]])

# We have 3 sets of weights - one set for each neuron
# we have 4 inputs, thus 4 weights
# recall that we keep weights transposed, in order to match
# up correctly.
weights = np.array([[0.2, 0.8, -0.5, 1],
[0.5, -0.91, 0.26, -0.5],
[-0.26, -0.27, 0.17, 0.87]]).T

# Sum weights related to the given input multiplied by
# the gradient related to the given neuron
dx0 = sum([weights[0][0]*dvalues[0][0], weights[0][1]*dvalues[0][1],
weights[0][2]*dvalues[0][2]])
dx1 = sum([weights[1][0]*dvalues[0][0], weights[1][1]*dvalues[0][1],
weights[1][2]*dvalues[0][2]])
dx2 = sum([weights[2][0]*dvalues[0][0], weights[2][1]*dvalues[0][1],
weights[2][2]*dvalues[0][2]])
dx3 = sum([weights[3][0]*dvalues[0][0], weights[3][1]*dvalues[0][1],
weights[3][2]*dvalues[0][2]])

dinputs = np.array([dx0, dx1, dx2, dx3])

print(dinputs)
>>>
[ 0.44 -0.38 -0.07 1.37]
```

- `dinputs` is a **gradient** of the **neuron function** *wrt* to **inputs**.

---

- Defined the **gradient** of the **subsequent function** (*dvalues*) $\to$ *row vector* 
  - Explain **shortly**
- Via **NumPy's** perspective.
  - alongside both **weights / values** being **numpy arrays**
    - Simplify the **dx0  > dx3** calculation
      - The **weights** array is **formatted** so the **rows** contain **weights** related to **each input** (weights for all neurons for the given input)
- Thus **multiply** them by the **gradient vector** directly.

```python
import numpy as np

# Passed-in gradient from the next layer
# for the purpose of this example we're going to use
# a vector of 1s, 3 derivatives - one for each neuron
dvalues = np.array([[1., 1., 1.]])

# We have 3 sets of weights - one set for each neuron
# we have 4 inputs, thus 4 weights
# recall that we keep weights transposed
weights = np.array([[0.2, 0.8, -0.5, 1],
[0.5, -0.91, 0.26, -0.5],
[-0.26, -0.27, 0.17, 0.87]]).T

# Sum weights related to the given input multiplied by
# the gradient related to the given neuron
dx0 = sum(weights[0]*dvalues[0])
dx1 = sum(weights[1]*dvalues[0])
dx2 = sum(weights[2]*dvalues[0])
dx3 = sum(weights[3]*dvalues[0])

dinputs = np.array([dx0, dx1, dx2, dx3])

print(dinputs)
>>>
[ 0.44 -0.38 -0.07 1.37]
```

- Achieve the same results by simply just using the **dot product**

- The **inner shapes** must **match** for this **to work**.

- The **shape** created must **match** the **subsequent function** 

  1. 1 PD $\to$ for **every neuron** 
  2. **Multiply** by the **neurons** *partial derivative* *wrt* **to the input** 
  3. Then **multiply** each of those **gradients** with **each** of the **partial derivatives**.
     - That are **related** to the **neurons input**
       - Which are **rows**.

- The **DP** takes **rows** from the **first argument** and **columns** from the **second**
  - To **perform multiplication** and **sum**.
    - This is why we **transpose** the **weights**.

  ```python
  import numpy as np
  
  # Passed-in gradient from the next layer
  # for the purpose of this example we're going to use
  # a vector of 1s
  dvalues = np.array([[1., 1., 1.]])
  
  # We have 3 sets of weights - one set for each neuron
  # we have 4 inputs, thus 4 weights
  # recall that we keep weights transposed
  weights = np.array([[0.2, 0.8, -0.5, 1],
                      [0.5, -0.91, 0.26, -0.5],
                      [-0.26, -0.27, 0.17, 0.87]]).T
  
  # sum weights of given input
  # and multiply by the passed-in gradient for this neuron
  dinputs = np.dot(dvalues[0], weights.T)
  
  print(dinputs)
  
  >>>
  [ 0.44 -0.38 -0.07 1.37]
  ```

- Account now for **batch sampling**
  - Have used a **single sample** so far for a **single gradient**
    - This is **backpropagated** *between* the *layers*.
- The **row vector**
  - We created for `dvalues` is **preparation**
- With **more samples**
  - The *layer* returns a **list of values** 
    - replace singular gradient `dvalues[0]` with **a full gradient list** `dvalues`. 
    - Add **more example gradients: **

```python
import numpy as np

# Passed-in gradient from the next layer
# for the purpose of this example we're going to use
# an array of an incremental gradient values
dvalues = np.array([[1., 1., 1.],
[2., 2., 2.],
[3., 3., 3.]])

# We have 3 sets of weights - one set for each neuron
# we have 4 inputs, thus 4 weights
# recall that we keep weights transposed
weights = np.array([[0.2, 0.8, -0.5, 1],
[0.5, -0.91, 0.26, -0.5],
[-0.26, -0.27, 0.17, 0.87]]).T

# sum weights of given input
# and multiply by the passed-in gradient for this neuron
dinputs = np.dot(dvalues, weights.T)

print(dinputs)

>>>
[[ 0.44 -0.38 -0.07 1.37]
[ 0.88 -0.76 -0.14 2.74]
[ 1.32 -1.14 -0.21 4.11]]
```

##### Derivatives of weights

- Calculate **gradients** *wrt* to **weights** is nearly the same
  - We are using **gradients** here to **update weights**
    - Therefore it must **match the shape** of *weights* rather than **inputs.**
- derivative *wrt* **weights** is **equal** to the **inputs**
  - Weights are **transposed** 
    - Therefore we **transpose** the **inputs** to obtain **derivative** of a **neuron** *wrt* to **weights**.
- Use these **transposed inputs** as *first parameter* of **dot product**
  - The **DP** $\to$ *multiplies* the **rows** by **inputs**. 
    - Each **row** $\to$ after **transposistion** $\to$ has **data** for **a given input** for *every sample*.
      - By **columns** of *dvalues*.
        - These **columns** $\to$ relate to **output** of **single neuron** for **all samples**
          - Result = **array** with **shape** the same as **the weights**.
          - Contains the **gradients** *wrt* to the **inputs** 
            - **Multiplied** by the **incoming gradient** for **all batch samples**.

```python
import numpy as np

# Passed-in gradient from the next layer
# for the purpose of this example we're going to use
# an array of an incremental gradient values
dvalues = np.array([[1., 1., 1.],
					[2., 2., 2.],
					[3., 3., 3.]])

# We have 3 sets of inputs - samples
inputs = np.array([[1, 2, 3, 2.5],
				   [2., 5., -1., 2],
                   [-1.5, 2.7, 3.3, -0.8]])

# sum weights of given input
# and multiply by the passed-in gradient for this neuron
dweights = np.dot(inputs.T, dvalues)

print(dweights)

>>>
[[ 0.5 0.5 0.5]
[20.1 20.1 20.1]
[10.9 10.9 10.9]
[ 4.1 4.1 4.1]]
```

- This **output** matches the **shape** of **weights**
  - This is because we **summed** the **inputs** for *every weight* 
  - We then **multiplied** via **input gradient**.
- `dweights` is a **gradient** of the **neuron function** *wrt* to **weights**. 

---

##### Derivatives of Bias

- For **biases** and the **derivatives** *wrt* to **them**
  - The **derivative** comes from **sum operation** and is **always** therefore **1**
    - Multiplied by **incoming gradients** to apply the **chain rule**.
- Gradients $\to$ are a **list of gradients** (*vector of gradients* for **every neuron** for **every sample**)
  - must **sum** them **with the neurons** 
    - **Column wise** along the **axis = 0**.

```python
import numpy as np

# Passed-in gradient from the next layer
# for the purpose of this example we're going to use
# an array of an incremental gradient values
# 3 neurons, 3 samples therefore a 3 x 3 array.
dvalues = np.array([[1., 1., 1.],
                    [2., 2., 2.],
                    [3., 3., 3.]])

# One bias for each neuron
# biases are the row vector with a shape (1, neurons)
biases = np.array([[2, 3, 0.5]])

# dbiases - sum values, do this over samples (first axis), keepdims
# since this by default will produce a plain list -
# we explained this in the chapter 4
dbiases = np.sum(dvalues, axis=0, keepdims=True)

print(dbiases)

>>>
[[6. 6. 6.]]
```

- Keep **gradient** as a **row vector** using *row vector*.

---

##### Derivative of ReLU function

- Equals **1** if **input** is $>0$ and $0$ otherwise.
  - The **layer passes** its *output* via a `ReLU()` activation during **forward pass**
- For **backward pass** $\to$ `ReLU()` receives a **gradient** of the **same shape**
  - The **derivative** of the `ReLU()` function forms **array** of **same shape**
    - Filled with **1** when **related input** $>0$ , 0 otherwise.
- To apply the **chain rule**
  - Must **multiply** this **array** with the **gradients** of the **following function**

```python
import numpy as np

# Example layer output
z = np.array([[1, 2, -3, -4],
[2, -7, -1, 3],
[-1, 2, 5, -1]])

dvalues = np.array([[1, 2, 3, 4],
[5, 6, 7, 8],
[9, 10, 11, 12]])

# ReLU activation's derivative
drelu = np.zeros_like(z) # creates a zero array of same shape as z
drelu[z > 0] = 1 # places 1 where z has values greater than 0
print(drelu)

# The chain rule
drelu *= dvalues # chain rule multiply the propogated d values.
print(drelu)

>>>
[[1 1 0 0]
[1 0 0 1]
[0 1 1 0]]

[[ 1 2 0 0]
[ 5 0 0 8]
[ 0 10 11 0]]
```

- Can **simplify** this operation
  - Since `ReLU()` derivative **array** is **filled** with `1's` 
    - These **dont change** the **values** multiplied by them
- Just take **gradients** of the **subsequent** function and **set to 0** , the values that **correspond** to the `ReLU()` input and are **equal** to or **less** than **0**.

```python
import numpy as np

# Example layer output
z = np.array([[1, 2, -3, -4],
[2, -7, -1, 3],
[-1, 2, 5, -1]])

dvalues = np.array([[1, 2, 3, 4],
[5, 6, 7, 8],
[9, 10, 11, 12]])

# ReLU activation's derivative
# with the chain rule applied
drelu = dvalues.copy() # ensure no modification of dvalues
drelu[z <= 0] = 0 # simply copy and overwrite values < 0 with 0.

print(drelu)

>>>
[[ 1 2 0 0]
[ 5 0 0 8]
[ 0 10 11 0]]
```

- Combine the **forward / backward pass** of a **single neuron** with a **full layer** and **batch based** *partial derivatives*
  - We shall **minimize** `ReLU` output for this example.

```python
import numpy as np

# Passed-in gradient from the next layer
# for the purpose of this example we're going to use
# an array of an incremental gradient values
dvalues = np.array([[1., 1., 1.],
[2., 2., 2.],
[3., 3., 3.]])

# We have 3 sets of inputs - samples
inputs = np.array([[1, 2, 3, 2.5],
				   [2., 5., -1., 2],
				   [-1.5, 2.7, 3.3, -0.8]])

# We have 3 sets of weights - one set for each neuron
# we have 4 inputs, thus 4 weights
# recall that we keep weights transposed
weights = np.array([[0.2, 0.8, -0.5, 1],
                    [0.5, -0.91, 0.26, -0.5],
                    [-0.26, -0.27, 0.17, 0.87]]).T

# One bias for each neuron
# biases are the row vector with a shape (1, neurons)
biases = np.array([[2, 3, 0.5]])

# Forward pass
layer_outputs = np.dot(inputs, weights) + biases # Dense layer
relu_outputs = np.maximum(0, layer_outputs) # ReLU activation

# Let's optimize and test backpropagation here
# ReLU activation - simulates derivative with respect to input values
# from next layer passed to current layer during backpropagation
drelu = relu_outputs.copy()
drelu[layer_outputs <= 0] = 0

# Dense layer
# dinputs - multiply by weights
dinputs = np.dot(drelu, weights.T)

# dweights - multiply by inputs
dweights = np.dot(inputs.T, drelu)

# dbiases - sum values, do this over samples (first axis), keepdims
# since this by default will produce a plain list -
# we explained this in the chapter 4
dbiases = np.sum(drelu, axis=0, keepdims=True)

# Update parameters
weights += -0.001 * dweights
biases += -0.001 * dbiases

print(weights)
print(biases)

>>>
[[ 0.179515 0.5003665 -0.262746 ]
[ 0.742093 -0.9152577 -0.2758402]
[-0.510153 0.2529017 0.1629592]
[ 0.971328 -0.5021842 0.8636583]]
[[1.98489 2.997739 0.497389]]
```

- Replaced **plain python functions** with **numpy variants**
- Created **example data**
- calculated **forward and backward passes**
- Updated the **parameters**
- Now update the **dense layer** and **ReLU activation code** with a **backward method**

```python
# Dense layer
class Layer_Dense:
    
# Layer initialization
def __init__(self, inputs, neurons):
	self.weights = 0.01 * np.random.randn(inputs, neurons)
	self.biases = np.zeros((1, neurons))

# Forward pass
def forward(self, inputs):
	self.output = np.dot(inputs, self.weights) + self.biases

# ReLU activation
class Activation_ReLU:
    
# Forward pass
def forward(self, inputs):
	self.output = np.maximum(0, inputs)
```

- During **forward** $\to$ for **Layer_Dense**
  - Wish to **remember** the **inputs** (*required* for calculating **PD** *wrt* weights **during backpropagation**)
    - Easily implemented by using an **object property** called **self.inputs**

```python
# Dense layer
class Layer_Dense:
...
# Forward pass
def forward(self, inputs):
...
self.inputs = inputs
```

- Then add **backward pass** $\to$ the actual **backpropagation**
  - That we **developed** previously to a **new method** in the **layer class**
    - Call this **backward**

```python
class Layer_Dense:
...

# Backward pass
def backward(self, dvalues):

    # Gradients on parameters
    self.dweights = np.dot(self.inputs.T, dvalues)
    self.dbiases = np.sum(dvalues, axis=0, keepdims=True)

    # Gradient on values
    self.dinputs = np.dot(dvalues, self.weights.T)
```

- Repeat same idea for the `ReLU` class.

```python
# ReLU activation
class Activation_ReLU:
    
# Forward pass
def forward(self, inputs):
    
    # Remember input values
    self.inputs = inputs
    self.output = np.maximum(0, inputs)

# Backward pass
def backward(self, dvalues):
    
    # Since we need to modify the original variable,
    # let's make a copy of the values first

    self.dinputs = dvalues.copy()

    # Zero gradient where input values were negative
    self.dinputs[self.inputs <= 0] = 0
```

- Everything required for backpropagation
  - Except for **derivative** of the **softmax activation function**
    - And the **derivative** of the **cross entropy loss** function.

##### Summary of Derivatives of ReLU / bias / inputs / weights

###### Inputs /weights

- First turn the **inputs** **partial derivative** *wrt* to the **ReLU** into the **simplified term**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210926142610413.png)

to

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210926142624348.png)

- This just takes the **subsequent layers** *dvalue* and **multiplies** by the **weight**
  - Ignoring if the **output** of **ReLU** is **less than zero**, as in make it 0.
- So there would be an a **2D array** of **dvalues** passed down from **layer to layer**
  - And then a **dot product is taken** to create a **derivative array** to backpropogate.
    - **2D array** as we work with **batches of samples** rather than a **single sample** of **derivatives** *passed down*.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210926145428426.png)

- The **dot product** takes in **dvalues** and **transposed weights**
  - The weights are stored as **transposed** (for **feed forward dot product**) so this just flips back to normal to **match** the **dvalues**.
- The only **difference with weights** is a **dot product** of the **transposed inputs** and **dvalues** is taken
  - each **row** *before transposition* in the **inputs** is a **sample** 
    - Thus **3 samples** of **4 inputs**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210926150537876.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210926145410224.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210926145441331.png)

- `drelu` being `dvalues` just **in practice** it would be **drelu for our simple function**.
- Recall the partial derivative of a **function** of **inputs and weights** *wrt* to a **weight** is the **input**, thus **multiplying by the weights / inputs** for *weights/ inputs respectively*

###### biases

- The Derivative is **just from sum operation**, and therefore is **1**
- Just **multiplied** by the **incoming gradients** from **subsequent function**
- Sum up the `dvalues` **column wise** as this represents **one sample** of **dvalues** for **every neuron** where there are **3 samples** of **3 neurons**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210926152338261.png)

###### Relu

- This takes **same shape** as the **dvalues** 
- Alters any **negative** values in the **array** to become **0**
- And that is it.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210926152458950.png)

---

- Then use this `drelu` for the `dinputs` / `dweights` /`dbiases` to **tweak values** and **backpropogate.** 

## Categorial Cross-Entropy Loss derivative

- Categorical Cross-Entropy loss function formulae is:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210922155250780.png)

- Where $L_i$ denotes **sample loss value**
- $i\text{ - }$ the **ith sample.**  
- $k$ - index of **target label** (*ground true label*)
- $y$ - **target values** 
- $\hat{y}$ - **predicted values**

---

- Formulae is **convenient**  when calculating the **loss value itself**
  - All that is required is the **output** of the **softmax activation function** at the **index** of the **correct class**
- For purpose of **derivative calculation**
  - Use the **full equation mentioned** in *chapter 5*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210922160907764.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210922161139957.png)

- Use the **full function** as the goal is **gradient calculation**
  - Compose of **partial derivatives** of the **loss function** *wrt* every **input.** 
    - Being **outputs** of the **softmax activation function**. 
- Therefore **cannot use the equation**.
  - Just takes **value** at the **index** of the **correct class** $\to$ *first equation above*
    - To **calculate** *partial derivatives* with **respect to** *each of the inputs*
      - Require an **equation** that **takes** *all of them as parameters*.
        - Therefore **choice** is to use **full equation**

- Define **gradient equation**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210924155833981.png)

- Defined equation here as **partial derivative** of the **loss function** *wrt* to **each of inputs**
  - We already **learned** that **derivative** of the **sum equals** the **sum of the derivatives**
    - Learned that we **can move constants**
- Example is $y_{i,j}$  $\to$ as Its **not what we are calculating** the **derivative** *wrt to*.
- Now Apply the **transforms**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210926132100523.png)

- Now solve the **derivative** of the **logarithimic function** 
  - This is the **reciprocal** of **its parameter** 
    - Via the **chain rule** $\to$ then **multiplied** by the **PD** of **this parameter**.
      - Using **prime notation** here:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210926180525188.png)

- Solve it **further** using **Leibniz's** notation in this case.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210926190611054.png)

- Then **apply this derivative**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210926180801260.png)

- The **PD** of a **value** *wrt* to this value **equals 1**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210926180827468.png)

-   Calculating **partial derivative** *wrt* to the **y** *given* *j*
  - The **sum** is thus **performed** over a **single element** and **can be omitted**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210926191356187.png)

- Full solution

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210926191520098.png)

- The **derivative** of this **loss function** *wrt* to **its inputs**

  > (**Predicated values** at the *i-th* sample as we are **interested** in a **gradient** *wrt* to the **predicted values**)

  - Equals the **the negative** *ground truth vector*
    - **Divided** by the **vector** of the **predicted values**
      - Which happens to be **the output vector** of the **softmax function**

---

### Implementation of the Categorical Cross-Entropy loss derivative 

- Simple **division** this **partial derivative**
- Therefore utilise **numpy** to perform **sample wise** vectors of **ground truth** / **predicted values**
- Further to perform **batch wise** (multiple like examples of the data features, samples are the taken from the same sort of area)

---

- From **coding perspective** $\to$ must add a **backward method** to the **loss categorical cross entropy class**
  - Must pass the **array of predictions** and the **array of true values** to it and **calculate** the **negated division of them**.

```python
# Cross-entropy loss
class Loss_CategoricalCrossentropy(Loss):
...

    # Backward pass
    def backward(self, dvalues, y_true):

        # Number of samples
        samples = len(dvalues)

        # Number of labels in every sample
        # We'll use the first sample to count them
        labels = len(dvalues[0])

        # If labels are sparse, turn them into one-hot vector
        if len(y_true.shape) == 1:
            y_true = np.eye(labels)[y_true]

        # Calculate gradient
        self.dinputs = -y_true / dvalues

        # Normalize gradient
        self.dinputs = self.dinputs / samples
```

- Along with the **partial derivative calculation**
  - Perform **two additional operations**
    - Turn the **numerical labels** $\to$ *one hot encoded vectors* 
- Prior to this $\to$ we **need to check** *how many dimensions* `y_true` consists of
  - If the **shape** of the **labels** returns a **single dimension** (list / not array)
    - Consist of **discrete numbers** thus must be **converted** to a **list** of **one hot encoded vectors** $\to$ *2d arrays*.
- Must turn these to **one hot encoded vectors**
  - Use `np.eye` method 
    - given number `n` , returns `nxn` array filled with **ones** on the **diagonal** and **zeros everywhere else**.

```python
import numpy as np
print(np.eye(5))

>>>
array([[1., 0., 0., 0., 0.],
[0., 1., 0., 0., 0.],
[0., 0., 1., 0., 0.],
[0., 0., 0., 1., 0.],
[0., 0., 0., 0., 1.]])
```

- Index with **numerical label** to obtain the **one-hot encoded vector** that *represents it*

```python
print(np.eye(5)[1])
>>>
array([0., 1., 0., 0., 0.])
print(np.eye(5)[4])
>>>
array([0., 0., 0., 0., 1.])
```

- if `y_true` is already **one hot encoded**
  - Do **not perform this step**

---

- Second **operation** is **gradient normalization**
- Next chapter $\to$ **optimizers** $\to$ will **sum all** of the **gradients** that are *related* to **each weight / bias** before **multiplying** them by **learning rate** (*other factor*)
- Our case $\to$ **more samples** in some *dataset* $\to$ the **more gradient** sets we **receive** at **this steps** thus **larger sum**
  - Must **adjust** *learning rate* according to **each set** of **samples**
    - Solve:
      1. **Divide** each gradient by **number of samples**
    - A **sum of elements** divided by the **count ** $\to$ *mean* 
      - Effectively normalize the **gradients** and make the **sums magnitude** *invariant* to the **number of samples**

---

## Softmax activation derivative

- Softmax **activation** function and its **partial derivative**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210927201839848.png)

- $S_{ij}$ $\to$ the `j-th` softmax output of `i-th ` sample.
- $z$ $\to$ **input array** which is a list of **input vectors** (*output vectors from previous layer*)
- $z_{i,j}$ $\to$ `j-th` softmax input of `i-th` sample
- $L$ $\to$ the **number of inputs**
- $z_{i,k}$ $\to$ `k-th` softmax input of the `i-th` sample.

---

- Softmax is the **exponentiated input** *divided* by the **sum of all exponentiated inputs**
  - Exponentiate all values $\to$ then **divide** each by **sum** of **all of them** to *perform* the **normalization** 
- Every **softmax input** will directly **impact** the **output**
  - Must calculate **derivative** of **each output** *wrt* to **each input**

---

- Programming side $\to$ calculate **impact** of **one list** on **another list**
  - Receive **matrix** of **values** as the **result**
    - Calculate the **==jacobian matrix==**  of **vectors**.

---

- Calculate **derivative** $\to$ define the **derivative** of the **division operation**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210928124305943.png)

- Start solving the derivative

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210928124330441.png)

- Apply **division operation**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210928124502985.png)

- There is **2 partial derivatives** now
  - One on the **right side** of **numerator** 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210928125539852.png)

- Calculate **derivative** of the **sum** of some **constant** *e*
  - Raised to $z_{i,l}$ (*l* denotes consecutive indices from $l$ to the **number of softmax inputs** $\to$ $L$ )
- With **respect** to the $z_{i,k}$ 
  - The **derivative** of the **sum operation** is the **sum of derivatives**
    - The **derivative** of the **constant** *e* raised to the **power** *n* with *respect* to `n` equals $e^{n}$

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210928131244768.png)

- Special case $\to$ when **derivative** of **an exponential function** equals this **exponential function**
  - Its **exp** $\to$ is **exactly** what we are **deriving** *wrt* to
    - Therefore its **derivative** is **equivalent** to `1`. 

---

- Range is $1$ to $L$ 
  - This contains **k** where *k* is one of the **indices** from this **range** $\to$ has this once.
    - The **derivative** is going to **equal** $e$ to the **power** of $z_{i,k}$ as $j$ equals $k$ 
      - And **0 otherwise**.
- When $j$ does **not equal** $k$ as $z_{i,l}$ will **not contain** $z_{i,k} and is **treated constant**
  - The **constant derivative** is **0**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210928132507626.png)

- The **derivative** on the **left side** of the **subtraction** *operator* in the **denominator** is **a different case**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210928215847827.png)

- This is **not the sum** 
  - Can either be $0$ if $j\neq{k}$ OR $e^{z_{i,j}}$ if $j=k$. 
    - Starting from **this step** $\to$ must calculate the **derivatives separately**  for **each case**

---

- $j=k$ 
  - The **derivative** on the **left side** is going to **equal** $e$ to the **power** of the $z_{i,j}$ 
    - The **derivative** on the **right solves** to the **same value**. 
      - Lets **substitute**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210928230153895.png)

- The **numerator** contains **the constant** $e$ to the **power** of $z_{i,j}$ twice
- Therefore **group it up**. 
- Also split up the **bottom**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210928232316890.png)

- Split into **two parts**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210928232532979.png)

- Moved $e$ from the **numerator** and **the sum** from the **denominator** to **its own fraction** , amongst other...
- Further **split** the **right fraction** into **two seperate fractions**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210928232819706.png)

- Left turns **fraction** turns **into softmax functions equation**
  - As well as the **right one** with the **middle fraction** of *solving* to $1$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210928233333300.png)

- The **left softmax** $\to$ carries the **softmax function** *carries* the $j$ parameter
- The **right** $\to$ $k$ 

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210928234607710.png)

- Go back to the case of $j\neq{k}$ 
- The **left** *derivative* of the **original** *equation* solves to $0$ as the **whole expression** is **treated** as **constant**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210929000257020.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210929000552785.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210929000606309.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210929000627706.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210929000735190.png)

- Summary solution of the **derivative**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210929000802641.png)

- This is **not the end**
  - When **left** in **this form**
    - There are **2 seperate equations** to code and **use in different cases**
      - This is **not convenient really**

- Morph the **second derivative**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210929001056272.png)

- Moved **second softmax** along the **minus sign** to **the brackets** 
  - So we **add a zero inside**, does **not change solution**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210929001931045.png)

- Both **solutions** are **similar** 
  - They **differ** in a **single value**
    - There exists the **==Kronecker delta==** function

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210929002131285.png)

- Can **apply it here** $\to$ simplifying the **equation further to**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210929002203086.png)

- This is the **final math solution** of the **derivative** of the **softmax function outputs** *wrt* **every input**.
  - Make it **easier** transform the **equation** *one more time*.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210929002928549.png)

- Basically multiplied it out

## Softmax activation derivative code implementation

- Make up a **single sample**:

```python
softmax_output = [0.7, 0.1, 0.2]
```

- Shape as a **list of samples**.

```python
import numpy as np

softmax_output = [0.7, 0.1, 0.2]

softmax_output = np.array(softmax_output).reshape(-1, 1)
print(softmax_output)

>>>
array([[0.7],
[0.1],
[0.2]])
```

- The **left hand side** is the **softmax output** *multiplied* by the **Kronecker delta**
  - The **KD** $\to$ equals $1$ when **both inputs** are **equal** and $0$ otherwise.
    - Visualise as an **array of zeros** with **ones** on the **diagonal**
      - May **remember** we *already implemented* such a **solution** via `np.eye` method.

```python
print(np.eye(softmax_output.shape[0]))
>>>
array([[1., 0., 0.],
	   [0., 1., 0.],
       [0., 0., 1.]])
```

- Do **multiplication** of **both of values** from the **equation part**

```python
print(softmax_output * np.eye(softmax_output.shape[0]))

>>>
array([[0.7, 0. , 0. ],
       [0. , 0.1, 0. ],
       [0. , 0. , 0.2]])
```

- Create speed by using `diagflat` that **creates** an **array** using an **input vector** as the **diagonal**

```python
print(np.diagflat(softmax_output))

>>>
array([[0.7, 0. , 0. ],
	   [0. , 0.1, 0. ],
	   [0. , 0. , 0.2]])
```

- The other **part** of the **equation** is $S_{i,j}S_{i,k}$ 
  - The **multiplication** of the **softmax outputs**
    - *iterating* over the `j` and `k` indices **respectively.** 
- For each sample $\to$ the **i index** 
  - We **multiply** the **values** from the **softmax functions** output (in *each combination*) 
    - Use **dot product** operation
      - Just have to **transpose** the **second argument** to obtain its **row vector** as described in **chapter 2**

---

```python
print(np.dot(softmax_output, softmax_output.T))

>>>
array([[0.49, 0.07, 0.14],
       [0.07, 0.01, 0.02],
       [0.14, 0.02, 0.04]])
```

- Perform the **subtraction** of **both arrays**

```python
print(np.diagflat(softmax_output) -
np.dot(softmax_output, softmax_output.T))

>>>
array([[ 0.21, -0.07, -0.14],
       [-0.07, 0.09, -0.02],
       [-0.14, -0.02, 0.16]])
```

- This results in a **==jacobian matrix==** 
  - An **array** of **partial derivatives** in *all* the **combinations** of **both input vectors**.
- We calculate the **partial derivatives** of *every output* of the **softmax function** *wrt* **each input** *seperately.* 
  - As **each input** *influences* **each output** via the **normalization process**
    - Takes the **sum of all** the **exponentiated inputs**
      - Result of this **on batch of samples** $\to$ a **list** of **jacobian matrices** forming a **3D matrix**.
        - Visualise as a **column** whose **levels** are **jacobian matrices** being the **sample wise gradient** of **softmax function**

---

- Question
  - If **sample wise gradients** are **jacobian matrices**
    - How do you **perform the chain rule** with the gradient **backpropagated** from the **loss function**.
      - As its a **vector** for **each sample**
        - What do you **do** with the **fact** that the **previous layer** which is the **dense layer** $\to$ expects **2D** but we pass **3D** list of **jacobian 2D matrices**.
-  The **derivative** of the **softmax function** *wrt* to *any input* returns a **vector of partial derivatives** for *each one*
  - Must **sum the values** from these **vectors** so *each* of **inputs** for **each** of the **samples** returns a **single partial derivative** instead 
    - As **each input influences** the **outputs**
      - The **returned vector** of **PD's** must be **summed** for the **final PD** *wrt* to **this input**
- Perform **this operation** on *every jacobian matrix directly*
  - Applying the **chain rule** at **same time** (applying **gradient** from the **loss function**) using `np.dot` for **each sample**.
    - Take the **row** from the **jacobian matrix** and **multiply** by the **corresponding value** from the **loss functions gradient**.
- As a **result** $\to$ the **dot product** of **each** of the **vectors** / **values** returns a **singular value**
  - Forms a **vector** of **partial derivatives** *sample wise* and a **2D array** (*list of resulting vectors*) **batch wise**.

###### Summary of this section above

- Just applied the **formulae** for the **partial derivative calculation**
- Special as **every input** will directly **affect** the **other values.**
- Each sample returns a **2D array**
- In batches therefore its a **3D** array of **jacobian ma**

---

```python
# Softmax activation
class Activation_Softmax:
...
# Backward pass
    def backward(self, dvalues):
        # Create uninitialized array
        self.dinputs = np.empty_like(dvalues)
        
        # Enumerate outputs and gradients
        for index, (single_output, single_dvalues) in \
        	enumerate(zip(self.output, dvalues)):

            # Flatten output array
            single_output = single_output.reshape(-1, 1)

            # Calculate Jacobian matrix of the output
            jacobian_matrix = np.diagflat(single_output) - \
            np.dot(single_output, single_output.T)

            # Calculate sample-wise gradient
            # and add it to the array of sample gradients
            # applys chain rule by multiplying the correct row of the
            # jacobian matrix with the correct d value to store in 
            # specifed index where the index is the sample number in
            # the batch
            self.dinputs[index] = np.dot(jacobian_matrix,
                                        single_dvalues)
```

1. Create an **empty array** $\to$ becomes the **resulting gradient array**.
   - has **same shape** as the **gradients** received by **applying chain rule**
   - `np.empty_like` creates **empty / uninitialized array**
     - Contains **meaningless values basically.**
     - Fill later.

2. **Iterate** *sample wise over pairs* of the **outputs** and **gradients** 
   - Calculating the **partial derivatives** as **described earlier** 
3. Calculate the **final product** by **chain rule** of **jacobian matrix** and **gradient vector**
   - From the **passed in gradient array**
     - Storing the **resulting** *vector* as a **row** in the `dinput` array.

- Store **each vector** in **each row** while *iterating* 
  - Forming the **output array.**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210930184404913.png)

- The *jacobian matrix* has **rows** 
  - Each row **calculated** is a **specific vector output** and stores the **derivative** of this output **wrt** to **every other input**.
  - Each column is therefore some **different output**
- In batches with **many samples** $\to$ we **recalculate** a *new jacobian matrix*
  - and store the **various values** in a final `dinputs` 

---

## Common Categorical Cross Entropy loss and softmax activation derivative

- Calculate **partial derivatives** of the **CCEL** and **Softmax activation function**
  - Actually **use them here**.
- Places mention the **derivative** of the **loss function** *wrt* to the **softmax inputs** / **weights / biases** of the **output layer directly**
  - There is **no detail** of the **partial derivatives** of the **functions seperately.** 
    - This is  as the **derivatives** of both combine to a **simple equation**
      - The **code implementation** is **faster/simple** to *execute* 
- Looking at **current code** $\to$ perform **multiple operations** to calculate the **gradients**
  - There is a **loop involved also** in the `backward` step of **activation function**.

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210930202041696.png)

- Apply **chain rule** $\to$ calculate the *partial derivative* of the **CCEL** 
  - *wrt* to **softmax function inputs**
    - Define **derivative** by *applying* the **chain rule**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210930191421107.png)

- This is the **PD** of the ==loss function== *wrt* to **its inputs** 
  - Then **multiplied** by via **chain rule** by the **partial derivative** of the **softmax** *wrt* to **its inputs**.
- We know the **inputs** to the **loss function** $\hat{y_{i,j}}$ are the **outputs** of the **softmax** $S_{i,j}$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210930191652658.png)

- This means **we update** the equation to **form**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210930191836732.png)

- Sub the **equation** for the **PD** of the **CCEF** 
  - We are **calculating** the **partial derivative** *wrt* the **softmax inputs**.
    - Must substitute an equation **containing** the **sum operator** *over* all of the **outputs**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210930192533918.png)

- After **substitution** to the **combined derivative** *equation*.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210930192624877.png)

- The **partial derivative** of the **softmax activation function**
  - before applying the **Kronecker delta**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210930192856349.png)

- Perform substitution of $S_{i,j}$ with $\hat{y}_{i,j}$. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210930193037977.png)

- Solution is **different** depending on the **two cases**.
  - To **handle this** $\to$ split the **current partial derivative** following these cases
    - When they **match** and **when they dont**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210930193241760.png)

- For the $j\neq{k}$ 
  - Just **updated** the **sum operator** to *exclude* the `k` , that is it.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210930193459164.png)

- For $j=k$ 
  - Do **not need the sum operator** as it will **sum a single element** of *index `k`*
    - For the **same reason** , also replace $j$ indices with `k` 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210930193557379.png)

- Return to **original equation**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210930193616493.png)

- Sub the **partial derivatives** of the **activation function** for **both cases** with **newly defined solutions**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210930193649489.png)

- Cancel out the $\hat{y}_{i,k}$ from **both sides** of the **subtraction** in the **equation**
  - Both **contain** it as **part** of the **multiplication** *operations* and *in their denominators*.
- On **right side** of the **equation** $\to$ replace the **2 minus signs** with the **plus one** and **remove parentheses**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211002143137598.png)

- Then **multiply** the $-y_{i,k}$ with the **content** of the **parentheses** on the *left* side of the equation:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211002143210841.png)

- Look at the **sum operation** $\to$ adds to $y_{i,j}\hat{y}_{i,k}$ over **every possible value** of *index* $j$ 
  -  Apart from when it **equals k**
    - On the **left hand** part there is $y_{i,k}\hat{y}_{i,k}$ 
      - Contains the $y_{i,k}$ $\to$ the **exact element** that is **excluded** from the **sum** 
        - **Join** *both* the **expressions** together.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211002143547204.png)

- The **sum operator** now *iterates* over **every possible value** of *j* 
  - We know that $y_{i,j}$ for *each* $i$ is the **one-hot encoded vector** of **ground truth values**
    - The **sum** of *all elements* equals to $1$.
- In another set of words
  - The **earlier explanation** in this **chapter** $\to$ this **sum multiplies** $0$ by $\hat{y}_{i,k}$ except for a **single situation**
    - The **true label** where it'll **multiply** $1$ by **this value**
      - Simplify **further**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211002143847305.png)

- Full solution:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211002143905031.png)

#### Summary of derivation

1. We wished to calculate the **partial derivative** of the **loss function** *wrt* to the **softmax inputs** $\to$ i.e the **neural networks** probability distribution vector. This therefore **combines** the **two equations** into **one simple calculation**.
2. Split this via the **chain rule** into as the **loss function** is `Loss(softmax(softmax_inputs))` 
3. Simplify the $S_{i,j}$ as its the **same** as **input** to **loss function** $\hat{y}_{i,j}$ 
4. We **calculated** the **derivative** of the **loss** already which was the **sum** shown above.
5. Break the **multiplication** into **two parts** dependent on the **i = k** or $i\neq{k}$ 
   - When its not equal its a **special case on outside** otherwise its **just every other one in the sum**.
6. Replace the **derivative** of the **softmax** with the **cases ** for each $i$  value
7. **Simplify down**
8. Expand out the LHS
9. Realises that the **sum ** can summate over **all the values now** as **we have a value** of the **sum** not within leaving just $-y_{i,k}$ on the **outside**
10. Finally realise the **sum** is a **truth value with one hot encoding** therefore only has a **single value** , which is the **one value we are looking for** where $i=k$ where $k$ varies on the **output truth value.**

---



## Common categorical cross entropy loss and softmax activation derivative - code implementation. 

- Code, **NO CHANGE** for the **forward pass**
  - Must be performed on **softmax activation** to **receive outputs** 
    - Then **on loss function** to **calculate the loss value**.
- For **backpropagation** $\to$ create a **backward step**
  - Contains the **implementation** of the **new equation** 
    - Which **calculates** the **combined gradients** of **loss / activation functions**.
- Code **solution** as a **seperate class**
  - Which **initializes** the **softmax activation** and the **categorical cross entropy object**.
    - Calling the **forward methods** *respectively* during the **forward pass**
      - Then the **new backward** *pass* contains:

```python
Softmax classifier - combined Softmax activation
# and cross-entropy loss for faster backward step
class Activation_Softmax_Loss_CategoricalCrossentropy():
    
    # Creates activation and loss function objects
    def __init__(self):
        self.activation = Activation_Softmax()
        self.loss = Loss_CategoricalCrossentropy()
    
    # Forward pass
    def forward(self, inputs, y_true):
        
        # Output layer's activation function
        self.activation.forward(inputs)
        
        # Set the output
        self.output = self.activation.output
        
        # Calculate and return loss value
        return self.loss.calculate(self.output, y_true)
    
    # Backward pass
    def backward(self, dvalues, y_true):
        # Number of samples
        samples = len(dvalues)
        
        # If labels are one-hot encoded,
        # turn them into discrete values
        if len(y_true.shape) == 2: 
        	y_true = np.argmax(y_true, axis=1) # selects the index  			which has the 1 in it.
            
        # Copy so we can safely modify
        self.dinputs = dvalues.copy()
        
        # Calculate gradient, uses an index array
        # to select every row basically [0,1,...,n] where n is 			    number of samples
        self.dinputs[range(samples), y_true] -= 1
        
        # Normalize gradient
        self.dinputs = self.dinputs / samples
```

- Implementing the **solution** of $\hat{y}_{i,k}-y_{i,k}$ 
  - Instead of **performing ** the **subtraction** of the **full arrays**
    - Take advantage of the **fact** that the $y$ being `y_true` in the code 
      - Consists of **one hot encoded** $\to$ means for **each sample** $\to$ there is a **singular value** of $1$ in **these vectors** 
        - The **remaining** positions are **filled with zeros**.

---

-  Means use **numpy** to **index** the **prediction array** with the **sample number** and **true value index**.
   - Subtracting $1$ from **these values**.
     - Requires **discrete** *true labels* instead of **one hot encoded ones** 
       - Additional code performs **transformation** if **required**.
-  Use `np.argmax()` $\to$ returns the **index** of the **maximum value** (*index* for 1 in **our case**)
   - Need to **perform** this operation **sample wise** to obtain a **vector** of **indices**.

https://numpy.org/doc/stable/reference/generated/numpy.argmax.html - argmax

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211005215327478.png)

- specify axis as its selects as default the flattened array which chooses the first indices if the others are the same which in this case is 0.

---

- Final step we **normalize** the **gradient** in same way and for **same reason** as **described** with the **Categorical cross entropy gradient normalization**

---

- Summary of code for **each classes** that have **updated**

##### Softmax activation final code

```python
# Softmax activation
class Activation_Softmax:
    
    # Forward pass
    def forward(self, inputs):

        # Remember input values
        self.inputs = inputs

        # Get unnormalized probabilities
        exp_values = np.exp(inputs - np.max(inputs, axis=1,
        keepdims=True))

        # Normalize them for each sample

        probabilities = exp_values / np.sum(exp_values, axis=1,
        keepdims=True)

        self.output = probabilities

    # Backward pass
    def backward(self, dvalues):

        # Create uninitialized array
        self.dinputs = np.empty_like(dvalues)

        # Enumerate outputs and gradients
        for index, (single_output, single_dvalues) in \
        enumerate(zip(self.output, dvalues)):

            # Flatten output array
            single_output = single_output.reshape(-1, 1)

            # Calculate Jacobian matrix of the output
            jacobian_matrix = np.diagflat(single_output) - \
            np.dot(single_output, single_output.T)

            # Calculate sample-wise gradient
            # and add it to the array of sample gradients
            self.dinputs[index] = np.dot(jacobian_matrix,
            single_dvalues)
```

##### Cross entropy loss code

```python
# Cross-entropy loss
class Loss_CategoricalCrossentropy(Loss):
	# Forward pass
    def forward(self, y_pred, y_true):
        # Number of samples in a batch
        samples = len(y_pred)

        # Clip data to prevent division by 0
        # Clip both sides to not drag mean towards any value
        y_pred_clipped = np.clip(y_pred, 1e-7, 1 - 1e-7)
        
        # Probabilities for target values -
        # only if categorical labels
        if len(y_true.shape) == 1:
            correct_confidences = y_pred_clipped[
            range(samples),
            y_true
            ]
        
        # Mask values - only for one-hot encoded labels
        elif len(y_true.shape) == 2:
            correct_confidences = np.sum(
            y_pred_clipped * y_true,
            axis=1
            )
            
        # Losses
        negative_log_likelihoods = -np.log(correct_confidences)
        return negative_log_likelihoods
    
    # Backward pass
    def backward(self, dvalues, y_true):
        # Number of samples
        samples = len(dvalues)

        # Number of labels in every sample
        # We'll use the first sample to count them
        labels = len(dvalues[0])

        # If labels are sparse, turn them into one-hot vector
        if len(y_true.shape) == 1:
            y_true = np.eye(labels)[y_true]

        # Calculate gradient
        self.dinputs = -y_true / dvalues

        # Normalize gradient
        self.dinputs = self.dinputs / samples
```

##### Softmax classifier - Combined with softmax activation and cross entropy loss for faster backward step

```python
class Activation_Softmax_Loss_CategoricalCrossentropy():
    
    # Creates activation and loss function objects
    def __init__(self):
        self.activation = Activation_Softmax()
        self.loss = Loss_CategoricalCrossentropy()
        
    # Forward pass
    def forward(self, inputs, y_true):
        # Output layer's activation function
        self.activation.forward(inputs)
        
        # Set the output
        self.output = self.activation.output
        
        # Calculate and return loss value
        return self.loss.calculate(self.output, y_true)
    # Backward pass
    def backward(self, dvalues, y_true):
        # Number of samples
        samples = len(dvalues)
        
        # If labels are one-hot encoded,
        # turn them into discrete values
        if len(y_true.shape) == 2:
        	y_true = np.argmax(y_true, axis=1)
            
        # Copy so we can safely modify
        self.dinputs = dvalues.copy()
        
        # Calculate gradient
        self.dinputs[range(samples), y_true] -= 1
        
        # Normalize gradient
        self.dinputs = self.dinputs / samples
```

- Test if the **combined backward step**
  - Returns the **same value** *compared* to when we **backpropogate gradients** through **both** of the **functions seperately**
    - For this example $\to$ *make up* some **output** of the **softmax function** and **some target values**
      - Then **backpropogate** using **both solutions**.

```python
softmax_outputs = np.array([[0.7, 0.1, 0.2],
							[0.1, 0.5, 0.4],
							[0.02, 0.9, 0.08]])

class_targets = np.array([0, 1, 1])

softmax_loss = Activation_Softmax_Loss_CategoricalCrossentropy()

softmax_loss.backward(softmax_outputs, class_targets)

dvalues1 = softmax_loss.dinputs

activation = Activation_Softmax()
activation.output = softmax_outputs
loss = Loss_CategoricalCrossentropy()
loss.backward(softmax_outputs, class_targets)
activation.backward(loss.dinputs)

dvalues2 = activation.dinputs

print('Gradients: combined loss and activation:')
print(dvalues1)
print('Gradients: separate loss and activation:')
print(dvalues2)
>>>

Gradients: combined loss and activation:
[[-0.1 0.03333333 0.06666667]
[ 0.03333333 -0.16666667 0.13333333]
[ 0.00666667 -0.03333333 0.02666667]]

Gradients: separate loss and activation:
[[-0.09999999 0.03333334 0.06666667]
[ 0.03333334 -0.16666667 0.13333334]
[ 0.00666667 -0.03333333 0.02666667]]
```

- They produce **same results**
  - The **small difference** of *values* in **both arrays** is from **precision** of the **floating point values** in *raw* Python and NumPy

---

- Use `timeit` to see **how much faster it is**
- Run **both solutions** *many times* 

```python
from timeit import timeit

softmax_outputs = np.array([[0.7, 0.1, 0.2],
[0.1, 0.5, 0.4],
[0.02, 0.9, 0.08]])
class_targets = np.array([0, 1, 1])

def f1():
    softmax_loss = Activation_Softmax_Loss_CategoricalCrossentropy()
    softmax_loss.backward(softmax_outputs, class_targets)
    dvalues1 = softmax_loss.dinputs
def f2():
    activation = Activation_Softmax()
    activation.output = softmax_outputs
    loss = Loss_CategoricalCrossentropy()
    loss.backward(softmax_outputs, class_targets)
    activation.backward(loss.dinputs)
    dvalues2 = activation.dinputs
    
t1 = timeit(lambda: f1(), number=10000)
t2 = timeit(lambda: f2(), number=10000)

print(t2/t1)

>>>
6.922146504409747
```

- Gradients seperately therefore **7 times slower**
  - This factor **differs** from **machine to machine**
    - But shows it was **worth putting effort** to *calculate* the **optimized solution** of the **combined loss** and **activation function derivative**,

---

- Look at **model code** and **initialize** the **new class** of **combined accuracy / loss class object**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211005225047633.png)

- Rather than **previous**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211005225105530.png)

- Replace **forward pass calls** over these **objects**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211005225134788.png)

- With **forward pass** on the **new object**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211005225221782.png)

- Add the **backward step** and **printing gradients**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211005225242476.png)

#### Full model code:

```python
# Create dataset
X, y = spiral_data(samples=100, classes=3)

# Create Dense layer with 2 input features and 3 output values
dense1 = Layer_Dense(2, 3)

# Create ReLU activation (to be used with Dense layer):
activation1 = Activation_ReLU()

# Create second Dense layer with 3 input features (as we take output
# of previous layer here) and 3 output values (output values)
dense2 = Layer_Dense(3, 3)

# Create Softmax classifier's combined loss and activation
loss_activation = Activation_Softmax_Loss_CategoricalCrossentropy()

# Perform a forward pass of our training data through this layer
dense1.forward(X)

# Perform a forward pass through activation function
# takes the output of first dense layer here
activation1.forward(dense1.output)

# Perform a forward pass through second Dense layer
# takes outputs of activation function of first layer as inputs
dense2.forward(activation1.output)

# Perform a forward pass through the activation/loss function
# takes the output of second dense layer here and returns loss
loss = loss_activation.forward(dense2.output, y)

# Let's see output of the first few samples:
print(loss_activation.output[:5])

# Print loss value
print('loss:', loss)

# Calculate accuracy from output of activation2 and targets
# calculate values along first axis
predictions = np.argmax(loss_activation.output, axis=1)

if len(y.shape) == 2:
    y = np.argmax(y, axis=1)
    
accuracy = np.mean(predictions==y)

# Print accuracy
print('acc:', accuracy)

# Backward pass
loss_activation.backward(loss_activation.output, y)
dense2.backward(loss_activation.dinputs)
activation1.backward(dense2.dinputs)
dense1.backward(activation1.dinputs)

# Print gradients
print(dense1.dweights)
print(dense1.dbiases)
print(dense2.dweights)
print(dense2.dbiases)
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211005230132449.png)

# Summary of everything to this point, going through code step by step, including redundant calculations

1. First create a **spiral dataset** which contains **n samples** , in our code, its **100**, for simplicity I will be discussing it as **3 samples**. Alongside we wish to have **each sample** specifying **3 different classes**

   - This gives us two values
     1. X $\to$ This is a **2D array** which represents the **batch** of **samples** we are training on. The **feature** we are recognising the **classes via** is a **pair of coordinates**
     2. y $\to$ Our **truth labels**. These identify the **class** associated with **said coordinate pair**, this is called a **sparse** vector, as opposed to **one hot encoding** which holds a **one** where the **correct value is** in the same shape.

   - For **3 samples**, this generates **9** pairs of coordinates
   - Each one has a **truth value** which holds the **class index** of which is **represents**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211020221619445.png)

2. Create dense layer **one** with **our 2 input features** and **3 output values** to feed to the **next layer / output** of our **neural network**.

   - This is established with some **default weights** generated by a **normal distribution** of gaussian distribution mean = 0 and variance = 1

   - The **shape of our weights** is $(2,3)$ as to match each **input coordinate pair** to **3 output values** , therefore a total of **6 weights for this dense layer**

   - Same idea for the **dense 2**
   
3. Create a **ReLU** activation layer for this **layer**

4. Create another **dense layer** for **3 input features** where these **features** are the **3 output features** of our **dense layer output** after being passed via an **activation**. 

5. Create our **softmax and entropy loss** function, this is simplified to **one method**

   1. It initializes the **activation softmax** and **categorical entropy loss** functions $\to$ explain later
   
6. Perform a **forward pass** of our **dense layer 1**

   - Takes in **our features 2D array**
   
7. Perform a **forward pass** of our **first activation** Relu function
   - Simply take the input and output the **value** if greater than 0 otherwise **0**.
   
8. Perform **forward pass** for **dense layer 2**

9. Pass **dense layer 2 output** to the **loss activation / softmax function** forward
   1. Takes in the **layer 2 outputs** and the $y$ sparse **truth vector** 

   2. Passes the **layer 2 outputs** to the **softmax activation forward**
      1. First obtain the **unnormalized probabilities** by **negating** the **largest value** from **each input** , this makes the **largest value** $0$ 
         - Prevents **dead neurons / large exploding numbers**
         - Makes output $-\infty$ to $0$ as its range, thus the **exponential** becomes between $0$ and $1$. 
         
      2. Output the probability as **the same shape** as **input** by using 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210331211842628.png)
          
          - This just takes each **value** and divides by the **sum** of **exponentiated outputs**
          - Purpose of **exponential** is that **higher inputs** create **higher outputs**
          - It is capped at $0$ to $1$ to focus on **change** more than **magnitude between each**.

   3. Pass the **softmax output** along with the **y_true** to the **loss function** for **entropy loss** calculation.

      1. This calls the **forward method** on our **loss** , 

         1. Within the **forward class** we first **clip** data 

            - Prevent **0 division errors** , clip both ends **to prevent mean** going towards **any value**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211020230259180.png)

         2. If the truth vector is **sparse** like our one

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211020230233202.png)

            - So `y_pred_clipped` is a **2D array** of outputs
            - We use the **range** of samples in this list to generate an array [0,1,2,3,..., n] where n is the **length samples**
            - On each row corresponding to the **indices** in this we **take the item** that is `y_true` in that row as `y_true` is an **index position** example y_true = [0,0,1] takes the **first item** from **row 0** and the **second item** from **row 3** 

         3. If the truth vector is **a one hot vector**

            - We create the `correct_confidences` by summing the **predictions** with the **one hot truth vector** , which essentially just **selects** the **value** that is **true** by **multiplying it by one** and wrong ones by **zero**
            - Summing this up removes the values forming a **1D** list of **confidences** as with sparse

         4. Calculate the **log loss** of the **confidences **and return this as the **loss array**.

            - Applies -log(x) to each **value** in the `correct_confidence` array
         - Log creates values closer to 0 as being **higher** thus **greater loss**, vice versa for 1.
         
      2. Calculate the **mean value of loss** to gain **overall value for loss** for the **entire batch**, then return this as the **loss value** as the **forward output** for the **softmax_loss method**.
   
10. Calculate predictions which take the **largest** value from the **output** of **last stage**

11. Calculate accuracy which is a **average** of where the **predictions** gave the **correct truth value**

    - Note before doing this we check if the **truth array** is **one hot encoded**
    - If so take the **argmax** which generates a **y array** with the **index** of the **correct** class as each **input**, then go onto the **calculate mean** accuracy of our model.

​             

## Backward pass now (backpropagation)

1. Take the **output of the loss function**, in sense being the **output** of the **NN** and **backward pass** starting with **softmaxloss combined** backward method

   1. Take in the **loss output** as `dvalues` along with the **truth values**

   2. If the **truth shape** is **one hot**, convert to **sparse** by taking the **argmax** along the **column axis**

   3. Copy the **dvalues** to `dinputs` as to **not change them accidentally**

   4. Apply the **derivative equation** derived earlier 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211002143905031.png)
      - This in code is displayed as:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211020234657448.png)

      - So our `dinputs` is just the **loss output**, and we go through **each sample** row

      - Take the index according to `y_true` 1D array holding the **index**

      - And then simply **minus one** as the formulae is **predicted values **

      - $\hat{y}_{i,k}$ is the **predicted value** and $y_{i,k}$ is the associated **truth value** which is just **one** so we **negate one** from this value to obtain the **derivative**

   5. **Normalize** the gradients as to make the **size of the sample** not affect **relative size**

      - Make it some value between **0 and 1**

   6. Return this as the **first dvalue** to be **passed down via chain rule**

2. Take **derivative values** from **loss activation** and pass to **dense 2** 

   1. Calculate `dweights` which via **partial derivative laws** is just **dot** of the **inputs** and **dvalues** 

   2. Calculate `dbiases` which just **adds** the **values** passed down from `dvalues` 

      - It sums **downwards** each of the **row**

   3. Calculate `dinputs` using same logic as `dweights` therefore multiplying by **weights** and **dvalues this time**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211021004326836.png)

      - For weights replace the $w_0$ with $x_0$ and its the same idea
      - Note this was from my notes about **ReLU** being the **dvalues** however take the drelu/dz part as generically being the **derivative** from the **subsequent layer**, whatever that may be, either ReLU / softmax_loss.
      - We **transpose the inputs / weights** as to consider **every sample** for **one specific neuron**, as the dvalues come in representing a **derivative** for each sample so we **apply the chain rule** for **each value** according to that sample passed down.

3. Pass to **activation from dense layer 2 output**

   - The **dvalues from here** is just the **same dvalues** but removal of the  < 0 values , removal as in they are **set to 0** by the **Relu derivative** backpropagation. 

4. Pass to **final dense layer** to repeat **dense layer 2** and obtain the **gradients** to update with.

---

### Seperate backward pass for softmax / entropy loss

#### Softmax

1. Create an **unitialized** array of same shape as **dvalues** passed from the **loss function**

2. Iterate over each **sample** output with its corresponding **dvalues** 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211021013919983.png)

   - single output

3. Create a **single output array** which is a **column vector** of **length of outputs** in the **network**

4. Calculate **jacobian matrix** as described in **derivative derivation**, view derivation in another place

   - See  how this multiplies the **outputs** with itself **transposed** to calculate the second bit
   - The first bit is just diagflat as the kronecker delta function is a np.eye() array
   - so they are gonna be multiplied along anyway so its easier to combine to one.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210929002928549.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211021013848137.png)

5. This Creates a **2D array** as the **result** of the **partial derivative** as its between **two vectors** as for **softmax** ,every **input** is considered with **respect** to **all the other inputs**
6. The `dinputs` is indexed in according to the **sample we are on** and the **value placed** is the **dot product of** that jacobian matrix with **a single set of dvalues** 
7. We return a 2D array same shape as output of **softmax** which contains in **each row** the **dinputs** for each **specific sample** to be passed down to the **layer below** and further **multiplied via the chain rule**

####  Cross Categorical entropy loss

1. If the **truth shape** is **sparse** , create a **one hot encoded vector**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211021005501698.png)

   - Creates a **diagonal ones** with the **length** of how many **neurons** there are
   - Then indexes into the **correct offset** via `y_true` to obtain the **y_true values**
   - So `np.eye` creates a **diagonal one array** , example if there are **3 outputs**,  there are 3 ones across it.
   - Then we index `y_true` which creates a **array** which at every **row** has the **row** in `np.eye` corresponding to the **index** of `y_true` as to **select the rows** of truth
     - Example if `y_true` = [0,0,1]
     - np.eye(labels)[y_true] would basically go through y_true, and select the **first row** for as its index 0, then first again, and then the second row, this can **extend** to the *length* of `y_true` which is the **number of samples** in total and each row is the **feature values** of the **samples**

2. Apply derivative we calculated earlier

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210926191520098.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211021005710595.png)

     - `dvalues` is of course in this case the **predicted values** from the **softmax output**

3. Then **normalize this output** by **dividing by** number of samples
