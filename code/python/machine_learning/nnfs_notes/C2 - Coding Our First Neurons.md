# Coding our first neurons

## Single neuron

- There is a single **neuron** with **three inputs**
  - when we **initialize** the *parameters* in the **neural network** $\to$ the *weights* are **random** and **biases** set to $0$ 
- Input is either $\to$ the **training data** or the *outputs* of **neurons** of the **previous layer**
  - Make up values to start as the **input for now**:

```python
inputs = [1,2,3]
```

- Each **input** requires some **weight** to be *associated* with it. 
  - Inputs $\to$ are the **data** we *pass* to the **model** to obtain the **desired outputs**
  - Weights $\to$ the **parameters** to *tune* later to **obtain** these results.
- **Weights** $\to$ are *one type* of value to **change inside** the *model* $\to$ during the **training phase**.
  - Alongside the **biases**
- The *values* for the **weights** / **biases** are what gets *'trained'* and make the **model** actually **work**.
  - We will produce **mock weights** for now
    - Example first input $\to$ at **index 0 ** $\to$ 1 $\to$ has weight = **0.2**
    - Second input $\to$ weight = **0.8**
    - Third input $\to$ weight = **-0.5**  
- The **input** and **weight** lists now are:

```python
inputs = [1,2,3]
weights = [0.2,0.8,-0.5]
```

- Next $\to$ the **bias**
  - At the **moment** $\to$ we model a **single neuron** with **three inputs**
- Modelling a **single neuron** $\to$ there is a **single bias**
  - As there is **one bias per neuron**
- The **bias** is the *additional tunable* value but **not associated** with *any input* in **contrast** to the **weights**
  - We will **select** a random bias of $2$ 

```python
inputs = [1,2,3]
weights = [0.2,0.8,-0.5]
bias = 2 
```

- The **neuron** *sums* each *input* $\to$ **multiplied** by that **inputs weight**
  - Then *adds* the *bias*
- All the **neuron does** $\to$ is take the **fractions** of **inputs** $\to$ these **fractions** / **weights**
  - Are the **adjustable parameters** then adds **another adjustable parameter** *bias* $\to$ then **outputs**
-  Calculated as such:

> ```python
> output = (inputs[0]*weights[0]+
> 		inputs[1]*weights[1]+
> 		inputs[2]*weights[2]+bias)
> ```
>
> 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326135814738.png)

- What would we **need** to change for **4 inputs** rather than **3** as shown
  - Next to it we need **another weight** to **multiply** the **input with**
    - Code for this new data would be :

```python
inputs = [1.0,2.0,3.0, 2.5]
weights = [0.2,0.8,-0.5,1.0]
bias = 2.0
```

- Can be depicted **visually as**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326141828934.png)

- All together in code, including the **new input** and **weight** 
  - Produce the **output**:

```python
inputs = [1.0,2.0,3.0, 2.5]
weights = [0.2,0.8,-0.5,1.0]
bias = 2.0

output = (inputs[0]*weights[0] +
		  inputs[1]*weights[1] +
		  inputs[2]*weights[2] +
          inputs[3]*weights[3] + bias)
```

## A layer of neurons

- **Neural networks** often have *layers* that *consist* of **$>1$ neuron** 
  - Layers are just **groups** of **neurons**.
    - Each **neuron** within the **layer** takes the **same input** $\to$ either **training data** / **previous layer**
      - Each contain a **unique** set of **weights** and its **own bias** $\to$ produces a **unique output**
      - The **layer output** $\to$ is a **set** of **each** of these **outputs** $\to$ *one per neuron* 
- Scenario with **3 neurons** in a **layer** and **4 inputs**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326142818172.png)

- We will **keep** the **initial** $4$ inputs and **set of weights** for the **first neuron** the *same* as we **are using so far**
  - We will add **2 additional** and *made up* set of **weights** alongside **2 additional biases** to form **2 new neurons** for a **total of 3**
    - The **layers output** $\to$ is going to be a **list of 3 values** , and **not just single** as with the **single neuron**

```python
inputs = [1, 2, 3, 2.5]

weights1 = [0.2, 0.8, -0.5, 1]
weights2 = [0.5, -0.91, 0.26, -0.5]
weights3 = [-0.26, -0.27, 0.17, 0.87]

bias1 = 2
bias2 = 3
bias3 = 0.5

outputs = [
	# Neuron 1:
	inputs[0]*weights1[0] +
	inputs[1]*weights1[1] +
	inputs[2]*weights1[2] +
	inputs[3]*weights1[3] + bias1,
	# Neuron 2:
	inputs[0]*weights2[0] +
	inputs[1]*weights2[1] +
	inputs[2]*weights2[2] +
	inputs[3]*weights2[3] + bias2,
	# Neuron 3:
	inputs[0]*weights3[0] +
	inputs[1]*weights3[1] +
	inputs[2]*weights3[2] +
	inputs[3]*weights3[3] + bias3
]

```

- Set of **three weights** and **three biases**
  - Defines **three neurons**
    - Each one is **connected** to the *same inputs*. 
      - This is a **fully connected neural network** $\to$ every *neuron* in the **current layer** $\to$ has a **connections** from every *neuron* from the **previous layer** $\to$ *COMMON* but **not required**
- Challenging to code **many layers** with this *manual method*. 
  - Use **loops** to *scale* the **dynamically sized inputs** and *layers*
    - We turned the **seperate weights** variable into a **list** of **weights** so we can *iterate* 
    - Changed code to use **loops** instead of **hardcoded operations**

```python
inputs = [1, 2, 3, 2.5]

weights = [[0.2, 0.8, -0.5, 1],
		   [0.5, -0.91, 0.26, -0.5],
		   [-0.26, -0.27, 0.17, 0.87]]

biases = [2, 3, 0.5]

#Output of current layer
layer_outputs = []

#For each neuron
for neuron_weights, neuron_bias in zip(weights, biases):
    #Zeroed output of given neuron
    neuron_output = 0
    
    #For each input and weight to the neuron
    for n_input, weight in zip(inputs, neuron_weights):
        #Multiply this input by the associated weight
        # add to the neuron output variable
        neuron_output += n_input * weight
    #Add bias
    neuron_output+= neuron_bias
    
    #Put neurons result to the layer's output list
    layer_outputs.append(neuron_output)
```

- How do we **know** we have **three neurons** / why do we have **three** ? 
  - Because we have **three sets of weights** and **3 biases**.
- Making your **own neural network** $\to$ decide **how many neurons** you wish for **each layer**
  - Can **combine** however many **inputs** you are **given** with **however many neurons** 

- Gain *intuition* on **how many neurons** to use throughout this book.
  - Start with using **trivial number** of neurons to aid **understanding within** this book at the core.

- **Tensor flow** gets it name from the various operations it does on **tensors** *multidimensional arrays* 

---

## Tensors, Arrays and Vectors

- Difference of these three is the **context** or **attributes** of the **tensor object**.
- A **python list** $\to$ comma separated **objects** within an array.

```python
l = [1,5,6,2] # list 1D array

# list of lists  2D array
l = [[1,2],[2,3],[4,5]]

# list of lists of lists 3D array
lolol = [[[1,5,6,2],
[3,2,1,3]],
[[5,2,1,2],
[6,4,8,4]],
[[2,8,5,3],
[1,1,9,4]]]
```

- All can become an **array representation** of a **tensor**
  - A list can do **whatever it likes** in terms of shape.

```python
anotherList = [[4,2,3], [1,2]]
```

- Can **not be an array** $\to$ its *not **homologous***
  - A **list of lists** is *homologous* if **each** list along the **dimension** is **identically long*
    - Must hold for **each dimension**.

- List above reading **across the row** is the **2nd dimension**
  - first list = 3 and second = 2 so **non-homologous**. 
- Also read **down** the **column dimension**. 
  - Every **dimension** does **not need same length** 
    - $4\times{3}$ is **fine**. 

---

- **Matrix** $\to$ a **rectangular array** 
  - Has **columns / rows** 
    - Its **two dimensional** therefore can be a **2D array**

- **Not all** *arrays* can be **matrices**
  - Arrays can be **far more** than simply **columns / rows** as it can go to any $n$ dimension it wishes. 

---

```python
list_matrix = [[4,2],
			   [5,1],
			   [8,2]]
```

- The **shape** of this array is $3\times{2}$ 
  - Formally $(3,2)$ at it has **3 rows** with **2 columns**. 
- To denote a **shape** we check **within the brackets**
  - **First dimension** $\to$ what is **inside** the **outer most brackets**
    - Above matrix $\to$ there are **3 lists** therefore **dimension** of $3$. 
  - **Second dimension** is the **number of elements** in the **inner most** brackets which is 2 therefore **dimension** $2$. 

```python
lolol = [
		[[1,5,6,2],
		[3,2,1,3]],
		
		[[5,2,1,2],
		[6,4,8,4]],
		
		[[2,8,5,3],
		[1,1,9,4]]
		]

```

- Contains **three matrices** 
  - its a **3D array** of shape $(3,2,4)$

---

- A **tensor** is an *object* that can be **represented** as an **array**
  - Treat tensors in the **context** of **deep learning**. 
    - They are **not just arrays** however are *represented* in the **form of arrays** to **programmers**.

---

- An **array** is an **ordered homologous** container for numbers
  - Use **this term** when working with **numpy** as its the **main data structure** within it. 
- **Linear array** $\to$ **1D** array is the **simple array** and in **python** is just a **list**.
  - Can be **multi dimensional** such as **matrices** which are **2D** arrays.
- **Vector** $\to$ be written as **list** or **array**
  - look at **vector algebraically** as a *set of numbers* in **brackets**.

## Dot Product and Vector addition

- Convert to **our python implementation** using **dot product**.
  - Traditionally used on **vectors**. 
    - Describe what we are doing as **tensors**.
- The neural networks are **like these objects** in a **complex multi dimensional vector space**. 

---

- **The dot product**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326191247256.png)

- Both vectors must be **the same size**

---

```
inputs = [1,2,3]
weights = [2,3,4]
```

- Apply the dot product to the weights and bias for an **efficient calculation.**
- **Addition** of the **two vectors** is an operation performed **element wise**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326191426236.png)

---

## Single neuron with NumPy

```python
import numpy as np

inputs = [1.0, 2.0, 3.0, 2.5]
weights = [0.2, 0.8, -0.5, 1.0]
bias = 2.0

# take the dot product
outputs = np.dot(weights, inputs) + bias

print(outputs)

```

## Layer of neurons with numpy

- The layer will be a **matrix** now or **2D array**. 
- Described the **dot product** of **two vectors**
  - But the **weights** are **now a matrix**
    - Must **perform** a **dot product** of them and **the input vector**.  
      - Numpy simplifies $\to$ treat the **matrix** as a **list of vectors** and **performing dot product** *one by one* with the **vector of inputs**
        - Returns a **list** of **dot products**. 

---

- **Dot product** result $\to$ a **vector / list** of **sums** of each **weight / input** product for *every neuron*.
  - Must add the **corresponding bias** 
    - These are **easily added** to the **result** of each **dot product** operation as they are **a vector** of the **same size**
      - Use **plain python** as **numpy** converts it **internally** to some **array**
- *Adding* **two vectors **$\to$ each $i^{th}$ element is **added together** resulting in a **vector** of the **same size**
  -  This is a **simplification** and **optimization** giving **simple / faster code**

```python
import numpy as np
inputs = [1.0, 2.0, 3.0, 2.5]
weights = [[0.2, 0.8, -0.5, 1],
		[0.5, -0.91, 0.26, -0.5],
		[-0.26, -0.27, 0.17, 0.87]]

biases = [2.0, 3.0, 0.5]
layer_outputs = np.dot(weights, inputs) + biases

```

- Whatever comes first decides the **output shape** of *np.dot*. 
  - *np.dot* treats the **matrix** as a **list of vectors** and performs a **dot product** of **each** of **those vectors** with the **other vector**
    - Used that property to **pass a matrix** $\to$ which was a **list of neuron weigh vectors** and a **vector of inputs** to obtain a **list of dot products** $\to$ neuron outputs. 

---

##  Batch of Data

- To **train** $\to$ **neural networks** receive data in **batches**
  - Example **input data** $\to$ has been a **single sample** / **observation** of *various features* called a **feature set**

```python
inputs = [1,2,3,2.5]
```

- Here the  [**1,2,3,2.5**] data $\to$ somehow **meaningful** and *descriptive* to the **output** that we **desire**
  - Picture **each number** as some *value* from a **different sensor** $\to$ from the **example** within **chapter 1** *simultaneously*
    - Each **value** is a **feature observation damn** $\to$ together $\to$ form a **feature set instance** called an **observation**. 
      - most commonly $\to$ **sample**.

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326222534837.png)

- Often **neural network** $\to$ expect to take **in many samples** at **time** for *two reasons*
  - One reason is that to **train in batches** in **parallel processing** $\to$ the other reason is that **batches** *help* with **generalization** during *generalization* $\to$ during **training**.

- If you **fit** (*perform a step of training process*) $\to$ on **one sample at a time** $\to$ you are **highly likely** to **keep fitting** to *that individual sample*
  - Rather **than** *slowly producing* general **tweaks** to *weights/biases* that **fit the entire dataset**.
    - *Fitting* or **training** in **batches** or *given* you **higher chance** of *making* more **meaningful changes** to **weights** and **biases**
      - For the **concept** of **fitment** in **batches** in *rather one* than **one sample at a time** $\to$ the *following animation*:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326224007567.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326225223901.png)

- Recall $\to$ **python** $\to$ *lists* are **useful container** for *holding*  a single **sample** as well as **multiple samples**
  - *Example* of a **batch** of **observations** each with *their own sample* $\to$ looks like:

```python
inputs = [[1,2,3],[2,3,4],[3,4,5]]
```

- The **list** of **lists** can be *made into* some **array** as its **homologous**. 
  - Each **list** within some **larger list** $\to$ is a *sample* of **representing** a **feature set**  $[1,2,3],[2,3,4]$ and $[3,4,5]$  are all **samples**. 
    - Also referred to as **feature set** of **instances** or *observations*

---

- We have a **matrix of inputs** and a **matrix** of **weights**
  - must perform the **dot product** on them.
- Must calculate the **matrix product**

## Matrix product

- Operation where there are **2 matrices** 
  - Perform **dot products** of all **combinations** of *rows* from the **first matrix** and **columns** of the **2nd matrix**.
    - Resulting in a **matrix** of **those atomic** *dot products*:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326230738636.png)

- The **shape** of the **resulting array** is always the **first dimension** of the **left array** $(4,7)$
  - and the **second dimension** $\to$ the **shape** of the **right array** $(5,7)$
- Above matrix $\to$ the **left matrix** has a **shape** of $(5,4)$ and the **upper right matrix** $\to$ has a **shape** of $(4,5)$
  - The **second dimension** $\to$ of the **left array** and the **first dimension** of the **second array** are *both 4* 
    - They **match** and the **resulting array** has a **shape** of $(5,5)$

---

- We can **show** that we can perform the **matrix product** on **vectors**. 
  - In **mathematics** $\to$ we can something called a **column vector** and **row vector**
    - Explain *better shortly* $\to$ they are **vectors** but **represented** as **matrices** with *one of the dimensions* having size $1$. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326231508854.png)

----

- $a$ is **row vector** $\to$ very similar to **vector** $a$ with an **arrow above it**
  - described **earlier** along with the **vector product**. 
- The **difference** in *notation* $\to$ between a **row vector** and **vector** are *commas* between **values**. 
  - And the **arrow** above *symbol* $a$ is **missing** on a **row vector**. 
- Called a **row vector** $\to$ as its a **vector** of **row** of *matrix* $b$ 
  - Alternatively $\to$ called a **column vector** $\to$ as its a **column of matrix**.
- As **row and column** are *matrices* $\to$ you do **not denote** them with **vector arrows** anymore

---

- When you **perform** the *matrix* **product** on them
  -  The **result** becomes a **matrix** like in the **previous example** but just contains a **single value**
    - The **same value** as in the **dot product example** we have looked at before. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326232502587.png)

---

- **Row** and **columns** *vectors* are *matrices* with **one dimension** being of a **size 1**
  - We **perform** the *matrix product* on them rather than the **dot product**.
    - Results in a **matrix** $\to$ containing some **single value**
- Shape (1,3) and (3,1) $\to$ create a **array** of *shape* (1,1) or size $1x1$

---

## Transposition for the matrix product

- Suddenly go from **2 vectors** $\to$ to *row* and *column* vectors
  - used **dot product / matrix product** saying a **dot product** of *two vector* equals a **matrix product** of *row / column vector*
    - The **arrows** above the **letters signify** that they are **vectors**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326233117285.png)

- Have used some **simplification** , not showing the **column vector** $b$ is actually **transposed** vector $b$ 
  - The **proper equation** $\to$ matching the **dot product** of *vectors* $a$ and $b$ written as **matrix product** should **look like**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326233408846.png)

- Here we **introduced** one *new operation* $\to$ **transposition** 
  - **Transposition** simply *modifies* a **matrix** in a **way** that the **rows** and **columns** become rows:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326233530645.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326233548276.png)

- A **row vector** $\to$ is a **matrix** whose *first dimension* *size* **number of rows** equals $1$ and the **second dimensions** *size* **number of columns** equals $n$ size.
  - Its a $1\times{n}$ array of **shape** $(1,n)$

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326233946527.png)

- In **numpy** with **3 values**
  - Define it as:

```python
np.array([[1,2,3]])
```

- Note use of **double brackets**
  - To *transform a list* to a **matrix** containing a **single row** (*perform equivalent operation of turning vector into **row vector***)
    - Put it into a **list** $\to$ create **numpy array**

---

```python
a = [1,2,3]
np.array([a])
```

- We **encase** $a$ in **brackets** before **converting** to an array **in this case**
  - Can **turn** it *into* a $1D$ array **expand dimensions** using the **numpy abilities**.

```python
a = [1,2,3]
np.expand_dims(np.array(a), axis =0)
```

- Where **np.expand_dims()** adds a **new dimension** at the **index** of the **axis**
  - A **column vector** is *a matrix* where the **second dimensions** size equals $1$. 
    - other word $\to$ **array of shape** $(n,1)$:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210327000111347.png)

- With **NumPy** it can be **created** the *same* way as **row vector**
  - But **needs** to *be additionally* **transposed** $\to$ *transposition* turns **rows** and **columns** into *rows*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210327000314838.png)

- Vector $b$ into **row** vector $b$ 
  - perform a **transposition** on it to form **column** *vector* $b$. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210327000506185.png)

```python
a = [1,2,3]
b = [2,3,4]

a = np.array([a])
b = np.array([b]).T

np.dot(a,b)
```

- We have **achieved** the *same result* as a **dot product** of **two vectors**.
  - Being performed **on matrices** and returning **some matrix**.
    - This is what **we wanted** and *expected* and **wanted**.
- Worth **mentioning** that *numpy* does not **have** a **dedicated method** for performing **matrix product**
  - The **dot product** and **matrix product** are both **implemented** within the **single method** of $np.dot()$ 

---

- **Matrix product** $\to$ on *two vectors* 
  - Took **one as is** $\to$ *transforming* into a **row vector**.
  - The **second one** $\to$ using *transposition* on it to convert to **column vector**.

---

## A layer of neurons and Batch of Data w/NumPY

- return to **inputs** and **weights** $\to$ when *covering* them
  - perform **dot products** on **all vectors** that *consist* of **both input /weight** *matrices*
    - As we have **just learned**
      - This this is the **operation** that the **matrix product** performs. 
- We need to **perform** *transposition* on **its second argument** 
  - This is **weights** *matrix* in our case
    - To **turn** the **row vectors** it *currently* consists of **column vectors**

---

- **Initially** we could **perform dot product** on the *inputs* and the **weights** with **no transposition** as the **weights** were a **matrix**
  - But the **input** were *just* a **vector**.
- In this case $\to$ the **dot product** $\to$ *results* in a **vector** of *atomic dot products* performed on **each row** from the **matrix** in **single vector**
  - When **inputs** $\to$ turn to a **batch of inputs** *a matrix*
    - must perform a **matrix product**.
      - Takes all **combinations** of *rows* from the **left matrix** and *columns* from the **right matrix** 
        - Performing a **dot product** on *them* then **placing** the results in the **output array**
- Both **arrays** $\to$ have the **same shape**
  - To **perform** the **matrix product** $\to$ the **shape's** value from **the index 1** of **first matrix** and **index 0** of the **second matrix** 
    - They **dont right now.** 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210327002848132.png)

- Look from **perspective** of **inputs/weights** 
  - Perform the **dot product** of *each input* and *each weight* in **all the combinations**
- The *dot product* takes the **row** from the **first array** and **column** from *second one*
  - But **currently** the **data** in **both arrays** are *row aligned* 
    - **Transposing** $\to$ the *second array* **shapes** the *data* to **be column-aligned**
      - The *matrix* product of **inputs** and **transposed weights** will **result** in a *matrix* containing **all atomic dot products** That we **need to calculate**
- The **resulting matrix** *consists* of **outputs** of all **neurons** after **operations** performed **on each input sample**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210327004014411.png)

---

```python
inputs = [[1,2,3,4],
         [3,4,5,6],
         [7,8,9,10]]
weights = [[11,12,13,14],
          [14,15,16,17],
          [17,18,19,20]]
biases = [3,4,5]

outputs = np.dot(inputs, np.array(weights).T) + biases

np.dot(inputs, np.array(weights).T)
```

---

- The **second argument** for $np.dot()$ is going to be the **transposed** weights
  - So the **first** thus is **inputs** $\to$ previously **weights** were the **first** *parameters*
- *Before* $\to$ we **were modelling** *neuron output* using **single sample batch** of *data* $\to$ a **vector**
  - Now we are a **step forward** 
    - When we **model** *layer behaviour* on a **batch of data**.
- We could **retain the** *current* parameter order
  - however **more useful** to have a **result** consisting of a **list** of **layer outputs** per **each sample** rather than **list of neurons** and their **outputs** *sample wise*
    - We wish the **resulting array** to be **sample related** and **not neuron related** as we **pass** them **samples further** through the **network** 
      - And the **next layer** will expect a **batch of inputs**.

---

- We **code this solution** with numpy
  - Can **perform** $np.dot()$ on **plain python** list of **lists** as **numpy** can *convert* them to **matrices**  *internally*
    - We are *converting* the **weights ourselves** , but to **perform ** $T$ $\to$ we *cant* on **normal** python list.
      - For **bias** $\to$ does **not ** require to be **numPy** array for the **same reason** that *numpy* is going to **do it internally**.

---

- **Biases** are a *list* $\to$ they are a **1D array** as a **NumPy array**
  - The *addition* of this **bias vector** to a **matrix** (*of the dot product* for this case)
    - Works **similarly** to the **dot product** of a **matrix** and *vector* that we **described earlier**.
- Bias **vector** $\to$ added to **each row vector** of the **matrix**
  - Each **column** of the **matrix product** result is an **output** of **one neuron**
    - The **vector** is **added** to *each row vector*
      - **First bias** added to **each first element** of **those vectors**, **second to second** etc.
        - That is all that is **required** $\to$ the **bias** of *each neuron* to **add** to **every result** of **this neuron performed** on *all input vector* (**samples**).

```python
inputs = [[1.0, 2.0, 3.0, 2.5],
			[2.0, 5.0, -1.0, 2.0],
			[-1.5, 2.7, 3.3, -0.8]]
			
weights = [[0.2, 0.8, -0.5, 1.0],
          [0.5, -0.91, 0.26, -0.5],
          [-0.26, -0.27, 0.17, 0.87]]

biases = [2.0, 3.0, 0.5]

outputs = np.dot(inputs, np.array(weights).T) + biases
```

- Implement what we have learnt to code

```python
inputs = [[1.0, 2.0, 3.0, 2.5],
          [2.0, 5.0, -1.0, 2.0],
          [-1.5, 2.7, 3.3, -0.8]]

weights = [[0.2, 0.8, -0.5, 1.0],
          [0.5, -0.91, 0.26, -0.5],
          [-0.26, -0.27, 0.17, 0.87]]

biases = [2.0, 3.0, 0.5]

layer_outputs = np.dot(inputs, np.array(weights).T) + biases

print(layer_outputs)
```

- Takes in **group** of **samples** and outputs a **group** of **predictions**
  - If you  have used any **deep learning libraries** $\to$ this is why you pass in a **list of inputs**
    - Even if its **just one feature set** and a **returned list of predictions** even if **one prediction**.

