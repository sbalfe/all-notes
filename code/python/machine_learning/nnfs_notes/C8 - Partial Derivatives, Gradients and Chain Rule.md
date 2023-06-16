[TOC]

# Gradients, Partial Derivatives and the Chain Rule

- Derivatives so far $\to$ depend on **single independent variable**. 
  - Result depended on solely on $x$ for **our case**.
- Our network consists of however:
  1. **Neurons** $\to$ each having **multiple inputs**.
  2. Every **input** gets **multiplied** by the: **weight** $\to$​ a function of **2 parameters**.
  3. Alongside a **bias summation** $\to$ a function of **as many parameters** as there are **inputs**, plus **one** for the **bias**.
- To learn the **impact** of all the **inputs / weights / biases**
  - To the **neuron output** at the **end** of the *loss function*
    - Must calculate the **derivative** of **every operation** that is performed in the **forward pass** in the **neuron / whole model**
- Use the **chain rule** in this case.

## The Partial Derivative

- Measures the **impact** that one **single input** has on some **functions output**
  - Same method of calculation as with **derivatives** 
    - *REPEAT* process for **every independent input**

---

- Every **functions input** $\to$ some **impact** on the **functions output** $\to$ even if **0 impact**.
  - Must **know these impacts**
    - This involves calculation of the **derivative** with **respect** to **every input** *separately*.
      - This is why they are **==partial derivatives==** as the *other inputs* are **constant**.

---

- The **partial derivative** is a *single equation* 
  - The complete **multivariate** functions **derivative** is a **set of equations** $\to$​ the **gradient**.
    - The **gradient** is a *vector* of the **size of inputs** that contain the **partial derivative solutions** with **respect** to **each ** of the **inputs**.

---

- To **denote** the **partial derivative** $\to$​ use *Eulers notation* 
  - Similar to **Leibniz's** $\to$ replace the **differential operator** $d$ $\to$ $\large \partial$ 
  - $d$ $\to$ can be used for **differentiation** of a **multivariate function** 
    -  The meaning is slightly different $\to$​ can mean the **rate of the functions change** in **relation** to **some given input**, but when **other inputs may change** as well, used in **physics**.

- We care about **partial derivatives** 
  - The situation where **we attempt** to find the **impact** of the **given input** $\to$​ **output**
    -  while treating **other inputs** as **constants**.

- Interested in the **impact** of **singular inputs** as **our goal** is to **update parameters** in the **model**
  - The $\partial$ operator means explicitly that the **partial derivative**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210818170843510.png)

### Partial Derivative of a Sum

- Calculating the **partial derivative** with **respect to a given input** means to calculate it like the
  **regular derivative** of **one input**, just while **treating other inputs as constants**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210818171325768.png)

- First apply **sum rule** $\to$ derivative of the sum is the **sum of derivatives**.
  - **derivative** of $x$ with respect to $x$ **equals 1**
  - New concept is $y$ with **respect** to $x$​ derivative. 
    - $y$ is treated as **constant** and does **not change** in *respect to $x$* 
      - Therefore $y$ becomes **0**.

---

- Second case is the **same idea** but with **respect** to $y$​ now.
  - Regardless of the **value** of $y$ , the **slope** of $x$ does **not depend** on $y$ 
    - NOT ALWAYS THE CASE AS SEEN BELOW.

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210818172140791.png)

- Apply **sum rule** first **of course**
  - We then **seperate** the **constants** away from the **derivatives**.
    - Calculate **derivatives** as normal with the **other value** set as **constant**
- Only **difference** to the **non multivariate derivatives** from the last part is the **partial part**
  - This means $\to$ we are **deriving** with **respect** to **each** of the **variables separately** 

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210818172653691.png)

- Same rules as always.

### Partial Derivative of Multiplication.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210818173531566.png)

- Treat independent variables as **constants** $\to$ move **constant** to **outside** of the **derivative**.
  - Therefore for first case it becomes **derivative of** $x$​ **multiplied** by **y**. 
    - Intuition $\to$ when calculating the **partial derivative** with *respect* to $x$ 
      - every **change in** $x$ $\to$ by **1** $\to$ changes the **functions output** by $y$.
- If $y=3$ and $x=1$ 
  - Result becomes $1\cdot{3}=3$ 
- Change the $x$ by **1** so $y=3$ and $x=2$ 
  - Result becomes $2\cdot{3}=6$ 
    - Changed the $x$ by **1** and the result changed by **3** $\to$ the $y$ 

---

- Adding a third variable

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210818174728999.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210818175307829.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210818175317095.png)

- Only new **operation** is as **mentioned**
  - Just moving the **variables** $\to$ other than **one** that we **derive** with **respect to** 
    - **OUTSIDE** of the **derivative.**

- Appear more **complex** but its just that the **constants** during the **derivation** 

---

- Purpose of **partial derivatives**
  - We calculate the **partial derivatives** of **multivariate functions** later $\to$ our **neurons**.
    - **Code perspective** $\to$ The **dense** layers **forward** method
      - Pass a **single variable** = **input array** that contains a **batch of samples** or **outputs** from the **previous layer** 
    - **Mathematical perspective** $\to$ every *value* of this **single array variable**
      - Is a **seperate input** $\to$contains as **many inputs** as **data** in the **input array**
        - Passing in a **vector** of **4 values** $\to$ its a **singular variable** in the **code**
        - But **4 seperate inputs** in the **equation** $\to$ forms a function with **multiple inputs** taken in.
- To learn of the **impact** that **each input** makes to the **functions output**
  - Calculate the **partial derivative** of **this function** with respect to  **each of the inputs**
    - Explained in later chapter.

---

### Partial Derivative of Max

- Not limited to **multiplication and addition** 
  - Must be derived for **other functions** that are used in the **forward pass**
    - one of which is the derivative of the **max()** function:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210818181930971.png)

- Max returns the **largest input**
  - We know the **derivative** of $x$ with respect to $x$ is **1**
    - So the **derivative** of the function with respect to $x$ **equals 1** if $x$ is **greater** than $y$ , as the function returns $x$. 

- Alternatively if $y >  x$ $\to$ returns **0** as the **derivative** with **respect** to a **constant** is just **0**
- Ignore the respect to $y$ as its **not required anywhere else**.

---

- The **special case** is where the `max()` only contains **one variable** parameter
  - And the **other parameter** is *always* some constant at `0`.
    - We return either **0** or the **larger value** $\to$ clips the value at **0** from the **positive side**. 

- Useful in calculation of the derivative for **ReLU** activation function as the **activation function** defined as
  - $max(x, 0)$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210818191620939.png)

- Takes **single parameter** therefore use the $d$ operator rather than the $\partial$​ operator.
- 1 when $x$ is **greater than 0** , otherwise its **0**. 

---

## The gradient

- The gradient is **a vector** composed of **all** the **partial derivatives** 
  - Calculated with **respect** to **each input variable**.
- Previous **partial derivative** calculation that was made **earlier**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210818193951402.png)

- Calculating the **partial derivative** with respect to **all of the parameters** we can form a **gradient** of the **function** 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210818194057989.png)

- This is the **gradient**
  - Just a **vector** of **all possible partial derivatives** of a function
    - Denote using the $\nabla$ symbol. **nabla**

---

- We are going to be **using** the **derivatives** of *single parameter* functions
  - And the **gradients** of **multivariate functions** to perform **==gradient descent==** using the **chain rule**
    - This is to form the **backward pass** $\to$ part of the **model training** 

## Chain Rule

- In the **forward pass** $\to$ pass the **data** through the **neurons**
  - Then through the **activation function**
    - Then through the **neurons** again in the **next layer**
      - Then *another* **activation function** .....

----

- We **call some function** with *some input parameter* 
  - Taking the **output** $\to$ using that $\to$ **input** to some **other function**
    - For the **simple example** $\to$ take **2 functions** $f$ and $g$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210818195312827.png)

- $x$ is the **input data** 
- $z$ is the **output** of the function $f(x)$ 
- Its an **input** for the **input** for the function $g$ 
- $y$ is an **output** of the function $g$ 

----

- Write same calculation as 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210818195453363.png)

- Not using $z$ as the **intermediate variable** 
  - $g$ directly takes the **output** of $f(x)$ as its **input** to produce some **output** $y$. 
- As $x$ is an **input** to the **function** $f$ and then this **output** of function $f$ is **input** to function $g$
  - The **output** of $g$ is **influenced** by $x$ 
    - There must exist some **derivative** that **informs** us of this **influence.** 

----

- The **forward** pass through **our model** is a **chain of functions** like these examples.
  - Pass in **samples** $\to$ data **flows** via **all the layers**, and **activation functions** to form an *output*
    - Bring the equation and the code of the example model from chapter 1

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210818200128527.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210818200148852.png)

- Look closely $\to$ presenting the **loss** as a **big function** / **chain** of **functions**
  - Each with **multiple inputs** 
    1. Input data
    2. Weights
    3. Biases

---

- Pass **input data** $\to$ to the **first layer** where we have the **layers weight / biases**
  - The **output flows** via the **ReLU** activation function, and **another layer**
    - Brings in more **weights / biases** and *another* **ReLU** $\to$ until the end $\to$ **output layer** and **softmax activation** 

----

- The **model output** , along with its **targets** $\to$ passed to the **loss function** 
  - Return the **model's error** 
    - Look at the **loss function** , not as one taking the **output** and **targets** as **parameters** to produce some **error**
      - But also as a **function** that takes:
        1. Targets
        2. Samples
        3. All weights, biases
      - As inputs if we **chain** all the **functions performed** during the **forward pass** as **shown**
- To **improve loss** $\to$ must learn how **weight / bias** impact it
  - To perform this derivative of a **chain of functions** $\to$ use the **chain rule. **

---

- The **derivative of  function chain**
  -  Is the **product** of **derivatives**  of **all the functions** in this chain. 
    - Example: 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210818202229670.png)

https://www.youtube.com/watch?v=FKraGDm2fUY - PROOF

- First we write the **derivative** of the **outer function** $\to$ $f(g(x))$ 
  - With respect to the **inner function** $\to$ $g(x)$ 
    - As the **inner function** is its **parameter**.
- Multiply by the **derivative** of the **inner function** $g(x)$ with *respect*  to its **parameters** $x$ 
- Denote the **derivative** using **2 different notations** of course.

---

- With **3 functions** and **multiple inputs**
  - The **partial derivative** of the **function** with *respect* to $x$ is **as follows**
    - Cannot use prime notation in this case as have to mention which variable we are deriving with respect to.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210818203058837.png)

- To calculate the **partial derivative** of a **chain of functions** with respect to **some inner parameter**
  - Take the **partial derivative** of the **outer function** with **respect** to the **inner function** in a **chain** to the **parameter**
    - Then **multiply** this **partial derivative** by the **partial derivative** of the **inner function** with respect to the **more inner function** in *respect* to the **other function** in the **chain**.
      - This is **repeated** down the **chain** ... 
- Notice $\to$ the **middle parameter** derivative is with **respect** to $h(x,z)$ and **not** $y$ as $h(x,z)$ is in the **chain** to the **parameter** $x$.
  - Essentially meaning its **not what we care about**, we are just looking for **respect** to $x$ 

---

- The **chain rule** is the **most important rule** in finding the **impact** of some **singular input** to the **output** of a **chain** of **functions**
  - This is the calculation of **loss** in **our case**.
    - Use again when discuss **backpropagation** 

## Summary

- Partial derivative of the **sum** with **respect** to *any input* equals 1

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210818204408076.png)

- Partial derivative of the **multiplication** operation with **2 inputs**
  - With respect to **any input** 
    - Equals the **other input**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210818204444211.png)

- The partial derivative of the **max function of 2 variables** with **respect to any of them** is **1** if this
  **variable** is the **biggest and 0** otherwise.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210818204519093.png)

- The derivative of the **max function** of a single **variable and 0 equals 1** if the **variable is greater**
  **than 0 and 0 otherwise**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210818204545938.png)

- The derivative of **chained functions** equals the **product** of the **partial derivatives** of the subsequent functions

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210818204624791.png)

- Same applies to the **partial derivative** 
  - Example. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210818204732784.png)

- The **gradient** is a **vector** of **all possible partial derivative**
  - Example of a **triple input function**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210818204832856.png)

