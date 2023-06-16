# functions

- Applied to the **output** of a **neuron** or even **layer of neurons**
  - This therefore **modifies the output**.
- Purpose of them is for example a **nonlinear** *activation function* $\to$ can enable **neural networks** with often 2+ **hidden layers** to **map** the **non linear functions**

---

- 2 **Activation functions**
  1. First $\to$ used in **hidden layers**
  2. Second $\to$ used in **output layer**

- Can be the same

---

## Step activation function

- The **purpose** that this **activation** function **serves** is to **mimic** a **neuron** *firing* / *not firing*. 
  - Based on **input information**.
- Step $\to$ outputs 1 if the value passed in is > 1 otherwise outputs 0.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210329040543478.png)

- The **activation** function has *historically been used* for **hidden layers** 
  - Nowadays $\to$ its **rarely a choice** 

---

## Linear activation function

- A **linear function** is one which is the **equation of a line**
  - Appear as a **straight line** of *graph* where $y=x$ 
- The **output** = **input**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210329040739677.png)

- Often applied to **last** layer in the **regression model**
  - a **model** that outputs a **scalar** value rather than a **classification** 
    - Cover **regression** in $C17$. 

---

## Sigmoid activation function

- A **step function** $\to$ *not very informative*
  - Reaching **training** and **network** *optimizers* $\to$ the way an **optimizer works** is it will **assess** each **individual** *impact* that the **weights / biases** have on the **network output** 
- Issue with **step** $\to$ it becomes **less clear** to the **optimizer** *what* the actual impacts are as there is **just** a single $1$ or $0$. 
  - Hard to tell how **close** the function is to **activation / deactivation**. 
    - May have been **extremely close** or even **very far**
      - For **final output** value from the network $\to$ this **does not matter** if it was **close** to *something else*
- Easier for the **optimizer** if there are **activation functions** with *granular* and **informative** values.

---

- The **original** *more granular* activation function used in **neural networks** $\to$ the **sigmoid activation function** 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210329041239781.png)

- Returns a **value** in the range of $0$ for **negative inf** through $0.5$ for **0 input** and $1$ for **positive infinity**

---

- With *dead neuron* $\to$ better for **granular approach** for the **hidden neuron activation functions**
  - We obtain a **value** in this case $\to$ that is **reversible** to the **original value** 
    - The **returned value** contains **all the information** from the *input*. 
      - Contrary to **function** such as the **step function** $\to$ output 3 = output 300,000.

- Sigmoid output being on a **range** of $0$ to $1$ $\to$ works far better **with neural networks** 
  - Especially **compared** to the **range** of the **negative** and **positive** *infinity*
    - Adds **non linearity** 
      - Importance $\to$ more important later.
- Sigmoid $\to$ eventually **replaced** by the **rectified linear unit** activation function
  - **Sigmoid** used as **output layer** later however.

## Rectified linear activation function

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210329052139498.png)

- If x <= 0 then output 0 otherwise output the **output** **linearly**.
- The **most widely used** activation function at time of writing for **various reasons** 
  - *Speed* / *Efficiency* reasons
    - While the **sigmoid** is **not the most complex** $\to$ its much more **challenging** to **compute** than **ReLU** activation function
- ReLU $\to$ so close to being a **linear activation function**
  - Whilst **remaining** a *non linear function* due to the **bend after 0**. 
    - This **simple property** is **very effective**

---

### Why use activation functions?

- For a NN to *fit a nonlinear function* $\to$ must **contain** at **least two** *hidden layers* and they must use a **nonlinear activation function**

---

-  A **non linear function** $\to$ can **not be represented** via some **straight line** such as **sine**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210329052700442.png)

- Some problems are **non linear** $\to$ most case in NN applications.
  - There are a **wide array of factors** that *come into play* 
    - Many **hard / interesting** *problems* are **non linear**. 

---

- Consider situation with **no activation functions**
  - **linear** essentially
    - With this in a NN of **2 layers** of **8 neurons** each $\to$ the **resulting** *training* of the model looks as so:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210329053023484.png)

- When using the **same** *2 hidden layers* and *8 neurons* with the **rectified linear activation function**
  - See the following:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210329053109324.png)

## Linear activation in hidden layers

- Consider **linear activation** on a **singular neuron level**
  - Given **values** for the **weights / biases** $\to$ what is the **output** for a *neuron* of $y=x$ activation
    - Lets look at examples $\to$ first try to **update** the **first weight** with some **positive value**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210329053311371.png)

- Continue to **tweak** the weights , **updating** with **negative numbers** this time

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210329053403739.png)

- **updating weights** and *additionally* a **bias**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210329053435379.png)

- Regardless of what we **do to the weights / biases**
  - The **output** of the **neuron** $\to$ is **perfectly linear** to $y=x$ 
    - This **linear nature** will *ripple* through the **entire network**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210329053548869.png)

- *Regardless* of **how many layers** $\to$ this network can **only depict** *linear relationships* if **using** some **linear activation function**
  - Entire network $\to$ becomes **linear**

## ReLU Activation in a pair of neurons

- **LESS OBVIOUS** how ReLU maps exactly to **non linear functions**
  - Mapping **non linear relationships** and **functions** is what i mean.

---

- Start with **singular neuron**
  - Begin with a **weight** $0$ and **bias** $0$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210329054000003.png)

- For **this case** $\to$ *regardless* of the **input** we *pass* 
  - The **output** of the neuron $\to$ is $0$ via $0$ weights
    - Alongside being **no bias**
- Set **weight** to $1$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210329054157925.png)

- Now it looks like a **basic** *ReLU* activation function
  - Set **bias** to $0.5$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210329054300252.png)

- In this case $\to$ for **singular neuron** the *bias offsets* the **overall function** *activation point* **horizontally**
  - Via **bias increase** $\to$ you make it **activate earlier** 
    - Setting the **weight** to be **negative 1**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210329054416111.png)

- The function has **become** a question of when it **deactivates** rather than **activates**
  - Previously we **discussed** on how to use the **bias** to *offset* the **function** in a *horizontal manner* and **weight** to *influence* the **slope** of the **activation**
- We can also **control** whether the **function** is one for **determination** of *activation/deactivation* position in the **function**.

---

- Case with **pair of neurons**
  - Pretend there is **2 hidden layers** of a **single neuron**
    - Thinking back to **y=x** *activation* > linear activation > linear results of course **regardless** of the **chain of neurons**

- For ReLU
  - Lets **begin** with the **last values** for the **1st neuron** and **weight** $1$ with **bias** $0$ for **2nd neuron**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210329054945048.png)

- So far $\to$ there is **no change**
  - The **2nd neuron bias** is **not offsetting** 
    - And the **2nd neuron** *weight* is just **multiplying** the *output* via $1$ 
      - Then we **adjust** the **2nd neurons** *bias* now. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210329055225771.png)

- The **bias** of the **second neuron** has *shifted* the **overall function** 
  - It shifted **vertically** rather **than horizontal however**
    - Try making **2nd neuron** *weight* be $-2$ rather than $1$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210329055400708.png)

- What we have $\to$ is a **neuron** that has both **activation** and **deactivation** points within the *function*
  - When **both neurons** are *activated* , when **their area of effect** comes *into play*
    - They **produce values** in the *range* of the **granular** / **variable** and **output**
- If **any neuron** in the **pair** is **inactive** $\to$ pair produce **non variable output**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210329060247818.png)

## ReLU Activation in Hidden Layers

- Use this **concept** $\to$ to **fit** the *sine wave function* 
  - Use **2 hidden layers** of **8 neurons each**.
    - Can *hand tune* the **values** to **fit the curve**.
- Do this by **working** with **one pair** of *neurons* at **any time**
  - Means $\to$ **one neuron** from *each layer individually* 
    - Going to **assume** the layers are **not densely connected** for *simplicity*. 
  - Every neuron from **first hidden layer** $\to$ connects to **one neuron** from **second hidden layer**
    - Often **not the case** in **real models** $\to$ for **simple purpose**
- The *example model* $\to$ takes **single value** as **input** 
  - The **input** to the **sine function** $\to$ output **single**
    - Output layer $\to$ use **linear activation** 
    - Hidden layers $\to$ use **ReLU** **activation**

---

- Start with **all 0 weights** , work with **first pair of neurons**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210329064045817.png)

- Can **set** the **weight** for the **hidden layer neurons**
  - And the **output** neuron to be $1$ $\to$ see how this **impacts** the **output**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210329064222479.png)

- This case $\to$ the **slope** of the **function** has been affected
  - Can **further increase** the **slope** via *weight adjustments* for the **first neuron** of the **first layer** to $6.0$. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210329064519654.png)

- Now see $\to$ for **example** $\to$ the **initial slope** of the function is **as we wish** 
  - A **problem** $\to$ this function **never ends** as the **neuron pair** does *not deactivate* at any **value**. 
    - We **visually** spot the area of **deactivation** 
      - Where the **red fitment line** (*NN output*) **diverges** from the *green sine wave* 
- We have the **correct slope**
  - We must set this spot as the **deactivation point**. 
    - Do this by **increasing** the **bias** for the *2nd neuron* of the **hidden layer** pair to $0.70$ 
      - This **offsets** the **overall** *function* in some **vertical manner**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210329064839493.png)

- You then **set the weight** for the *2nd neuron* to $-1$ 
  - Causes a **deactivation** *point* to **occur** $\to$ as its **horizontally** where we wish it to be.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210329064942150.png)

- We need to **flip this slope** *back*
  - How to **flip neuron output** 
    - We can **take** the **weight** of the *connection* to **output of neuron**
      - currently at $1.0$ and just **flip to -1** $\to$ **flips the function**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210329065103283.png)

- Require some **offset** 
  - For **hand optimization** $\to$ going to use **first 7 pairs** of *neurons* in the **hidden layers** to *create* the **sine waves** *shape*
    - The **bottom pair** is to **offset** *everything vertically*. 
- Set bias of **2nd neuron** in *bottom* pair to $1.0$ and **weight** to the **output neuron** as 0.7 
  - Can **vertically shift** the **line as such**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210329065332474.png)

- We have **completed** the *first section* with an **area of effect** being the **first upward** *section* of the **sine wave**
  - Start on **next section** that we wish
    - Start via setting the **weights** for this **2nd pair** of *neurons* to $1$ , this **includes** the **output neuron**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210329065525623.png)

- The **2nd pair** of *neurons* $\to$ activation *begins too soon*
  - This impacts the **area of effect** of the **top pair** that was **already aligned** 
- We want the **second pair** to *start influencing* the **output** where the **first pair** *deactivates*
  - We want to **adjust** the **function** *horizontally* $\to$ *adjust **bias***.
- To **modify** the *slope* $\to$ set the **weight** coming into the **first neuron** for the *2nd pair*
  - Set to $3.5$.
- Same method to **set slope** for the **first section**
  - *Controlled* by the **top pair** of *neurons* in the **hidden layer**
    - After the **adjustments**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210330033221047.png)

- Use **same method** as we did for **first pair** to *set* the **deactivation point**.
  - Set the *weight* for the **2nd neuron** in the **hidden layer** *pair* to $-1$ and the **bias** to $0.27$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210330033537111.png)

- Then **we flip** *this sections function*
  - The *same* way **we did** the *first one* 
    - Set the **weight** to the **output** of **neuron** $1.0$ to $-1.0$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210330033736161.png)

- Like first pair $\to$ use the **bottom pair** to *fix* this **vertical offset**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210330033809182.png)

- We **just continue** the *methodology* 
  - Leave it **flat** for the **top section**
    - Means it only **begins** to *activation* for the **3rd pair** of **hidden layer neurons** when you wish for **slope** to *start going down*.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210330033916149.png)

- Process is **repeated** for **each section**
  - Producing the **final result**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210330034337667.png)

- Then **begin** to *pass data* throughout the **neurons**
  - Observe how these *neurons* **area of effect** *comes into play*
    - Only when **both neurons** are *activated* based **on input**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210330034553236.png)

- $0.08$ $\to$ only *pair* activated is the **top** (*both green*) = output of variable nature
  - This is **their area of effect**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210330034703612.png)

- Only **fourth pair** of *neurons* 
  - With **no other weights** $\to$ used *crude properties* of a **pair of neurons** with **ReLU** *activation* to **fit the sine** well
- When you **enable** all the *weights* then enable a **mathematical optimizer** to *train*
  - View **greater fitment**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210330034927274.png)

---

- This explains purpose of **multiple** layers of neurons to fit **non linear functions** of which **ReLU** enables us to **map too**.
  - Further example $\to$ take above examples with **2 hidden layers** of **8 neurons each** and rather use **64 neurons** for each **hidden layer**
    - Seeing **greater** *improvement*:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210330035132533.png)

---

## Implementation

```python
inputs = [ 0 , 2 , - 1 , 3.3 , - 2.7 , 1.1 , 2.2 , - 100 ]
output = []
for i in inputs:
	if i > 0 :
		output.append(i)
	else :
		output.append( 0 )
print (output)
```

- Written simpler

```python
inputs = [ 0 , 2 , - 1 , 3.3 , - 2.7 , 1.1 , 2.2 , - 100 ]
output = []
for i in inputs:
	output.append(max(0,i))
    
print (output)
```

- Numpy version

```python
inputs = [ 0 , 2 , - 1 , 3.3 , - 2.7 , 1.1 , 2.2 , - 100 ]
output = np.maximum(0,inputs)
print (output)
```

- Compares **each element** of the *input* list or **an array**
  - Then **returns an object** of the *same shape* filled with the **new values**
    - Use this in our **rectified linear activation class**

```python
# ReLU activation
class Activation_ReLU :
	# Forward pass
	def forward ( self , inputs ):
		# Calculate output values from input
		self.output = np.maximum( 0 , inputs)
```

```python
# Create dataset
X, y = spiral_data( samples = 100 , classes = 3 )
# Create Dense layer with 2 input features and 3 output values
dense1 = Layer_Dense( 2 , 3 )
# Create ReLU activation (to be used with Dense layer):
activation1 = Activation_ReLU()
# Make a forward pass of our training data through this layer
dense1.forward(X)

# Forward pass through activation func.
# Takes in output from previous layer
activation1.forward(dense1.output)

# Let's see output of the first few samples:
print (activation1.output[: 5 ])
```

- The **negative** values are just **clipped**
  
  - That is all there is to be **used** within the **hidden layer**
  
  
## SoftMax activation function

- Model $\to$ must be a **classifier**
  - The **activation** function must be considering the **classification nature**
    - An example is the **softmax activation function**
- The ReLU $\to$ **unbounded** and *not normalized* with **other units**
  
  - It is said to be exclusive $\to$ its **not normalized** as in it can be **multiple things** such as 21/38/3490 etc.
- **Exclusive** $\to$ means each output is **independent** of *all others*
  - The softmax $\to$ takes in **non-normalized** / **uncalibrated** inputs
    - Produces a **normalized** *distribution* $\to$ of probabilities for **our classes**
- For **classification** $\to$ we see what the **network** *thinks* the **input represents**
  - The distribution returned by **softmax** $\to$ represents the **confidence score**
    - for *every class* $\to$ this **adds to 1**
      - the **predicted class** $\to$ is *associated* with the **output neuron** with the **greatest confidence score** 

- We note the **other confidence scores**

  - If the network has a confidence distribution for **two classes** such as [0.45, 0.55]
    - The **prediction** is the **2nd class** but the *confidence* is *low*
      - The program **may not act** for this case as its so low.

- Function for **softmax**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210331211842628.png)

- break it down in python
  
  - Example outputs from a neural network layer

```python
layer_outputs = [4.8, 1.21, 2.385]
```

- Apply exponent with **eulers number** $e$ 
  - this is roughly $2.718$ 
    - Exponentiating is **taking this constant** to the **power** of *some given parameter* 

$$
y=e^x
$$

- Both **numerator** and **denominator** of the *softmax* contain $e$ raised to some power of $z$
  - Where $z$ given some **indices** means the **singular output value**
    - The index $i$ means the **current sample** and the *index* $j$ means the **current output** in **this sample** 
- The **numerator** $\to$ *exponentiates* the **current output value**
  - The **denominator** takes a **sum of all** of the **exponentiated outputs**  of the **current output value**
- We calculate these exponentiates to continue



```python

#values from the previous output when we described what a neural network was

layer_outputs = [4.8, 1.21, 2.385]

e = 2.71828182846

# For each value within the vector, calculate the exponential value
exp_Values = []
for output in layer_outputs:
   exp_values.append(e**output)
print (exp_values)
```

- The purpose of this **exponentiation** is *varied*
  - we require values that are **non negative**
- A **negative probability** does *not make much sense*
  - An **exponential value** of *any number* $\to$ is **always non negative** 
    - Returns the value of $0$ for **-infinity** and $1$ for the **input of 0**, then infinitely increasing
- Its a **monotonic function**
  - With **higher input values** $\to$ the values are **higher**
    - We **wont change** the *predicted class* after **applying** it while ensuring that we obtain **non negative values**. 
- Adds sense of **stability** to the result
  - the **normalized exponentiation** more on the ==**differences** between numbers rather than **magnitudes**==
- Once we have **exponentiated** $\to$ convert the numbers to a **probability distribution**
  - This is *converting* the **values** into the **vector of confidences** 
    - With **one for each class**
      - this adds up to the value of 1.
- We are essentially performing an **normalization**
  - take the **given value** then **divide** it by the **sum of all values**
- For the **outputs** $\to$ which are all **exponentiated at this stage**
  - This is what the softmax function describes
    - Take some **exponentiated** value and **divide** this by the **sum of all exponentiated values**
- Each **output value** $\to$ *normalizes* to a **fraction of the sum**
  - Every single value is now within the range of $0$ to $1$ and add up to the value of 1.
    - They **share the probability** of $1$ between themselves
      - We now add the **sum** and **normalization** to the **code**

```python
# We now normalize the values
norm_base = sum(exp_values) # sum each value

norm_values = []
for value in exp_values:
    norm_values.append(value/norm_base)
print(norm_values)   

print('sum of normalized values', sum(norm_values))
```

- Perform the **same set of operations**
  - in numpy

```python
import numpy as np
# values from the earlier previous when we described what a NN was

layer_outputs = [4.8, 1.21, 2.385]

# For every value in a vector > calculate its exponated value

exp_values = np.exp(layer_outputs)

print(exp_values)

norm_values = exp_values / np.sum(exp_values)

print(norm_values)
print('sum of normalized values', np.sum(norm_values))
```

- The results are very similar
  - However they are faster to calculate and the **code** becomes **easier to read**
- To train within **batches**
  - need to **convert** this **functionality** to accept the **layer outputs** within **batches**
    - Doing this is **easy** as

```python
# Get unnormalized probabilities
exp_values = np.exp(inputs)

# Normalize them for each sample
probabilities = exp_Values / np.sum(exp_values, axis = 1, keepdims = True )


```

- We have **some function** 
  - Specifically np.exp $\to$ does the **exponential** $\to$ E**output part
    - Should also **address** and what *axis* and *keep dims* mean in the above.
      - First discuss the axis $\to$ axis is **easier** to **show** the **tell**
        - But in $2D$ array / matrix , axis = 0 refers to rows and axis 1 refers to columns
- Examples of how the axis **affects** the **sum** using **numpy**
  - First show the **default** which is **none**.

```python
layer_outputs = np.array([[4.8, 1.21, 2.385],
                         [8.9, -1.81, 0.2],
                          [1.41, 1.051, 0.026]])
print(np.sum(layer_outputs))

```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210402234338480.png)

- With **no axis** specified $\to$ you are just **summing** all of the values
  - Even if they **vary in dimensions**
- For $axis=0$ 
  - This means that we sum **row wise** along **axis 0**
- Other words $\to$ the **output** has the **same size** at this axis
  - As at **each of** the *positions* the **values** from *all other dimensions* at this **position** are *summed* to **form it**.
    - For our **2d array** $\to$ where we have a **single other dimension** *columns* 
      - The **output vector** will *sum these columns* 
        - Means we are **performing** the *calculation* $4.8+8.9+1.41$ 

```python
print('another way to think of it w/ a matrix == 0: columns: ')
np.sum(layer_outputs, axis = 0)
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210403000115155.png)

- We need **sums** of the **rows**
  - switch axis to be $1$. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210403000314460.png)

- We obtain the **wrong shape** we wish to **simplify** the **outputs** in a **single value per sample**
  - We are trying to **sum** all the **outputs** from a **layer** for *every sample* within in a batch
    - **Converting** the **layers** output array with **row length** that is **equal** to the **number** of **neurons** in *the layers* to **just one value**
- We require a **column vector** with *these values* as it enables us to **normalize** the *whole batch* of **samples**
  - *Sample wise* in **single calculation**

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210403000908866.png)

- We thus keep the same dimensions as thee input
  - If we **divide** the **array** containing a **batch of inputs** with this array
    - Numpy performs **this sample wise** $\to$ it will **divide** all of the values **from each output** row by the **corresponding** from the **sum array**
      - Since the **sum** in **each row** is a **single value** $\to$ used for the **division** with **every value** from *the corresponding output row*
        - Can **combine** all of this to **softmax class**:

```python
class Activation_Softmax:
	# Forward pass
	def forward(self, inputs):
    
        # Get unnormalized probabilties
        exp_values = np.exp(inputs-np.max(inputs, axis = 1, keepdims=True))

        # Normalize them for each sample
        probabilties = exp_values/np.sum(exp_values, axis = 1, keepdims = True)

        self.output = probabilties
    
```

- We include a **subtraction** of the **largest** of the *inputs* before we did the **exponentiation**
  - There are **two main pervasive challenges** in *neural networks*
    1. *dead neurons*
    2. **very large numbers** $\to$ the *exploding values*
- Dead neurons and enormous numbers **wreak havoc** down the **line** and thus **render** the **network** to become **useless** over time
  - The **exponential function** used in **softmax** is one of the **sources** of this *explosive value*

##### Examples.

- 1 > 2.718...
- 10 > 22026
- 100 > 2.688e+43
- 1000 > INF

---

- It **does not take** the *very large number* 
  - A mere 1000 will break the system causes an **overflow error**
    - We know the **exponential** functions tends towards 0 as the **input** value approaches **negative infinity** 
      - Output = 1 on input 0.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210403002949812.png)

- Use this property $\to$ to **prevent exponential functions** from **overflowing**
  - If we **subtract** the *largest value* from the **list of input values**
    - We then change the output values to always be within range of **some negative value** between from -inf  to 0
      - As the **largest number subtracted from itself** will of course become 0 and therefore output 1.
-  All numbers are between 0 and 1 now.
- Thanks the **normalization** of the values $\to$ this works fine and **does not change the output**.

```python
softmax = Activation_Softmax()

softmax.forward([[1,2,3]])
print(softmax.output)
softmax.forward([[ - 2 , - 1 , 0 ]]) # subtracted 3 - max from list, same as this 
```

- This is **another useful property** of the *exponentiated* and **normalized functions**
  - There is one more thing to **mention** in addition to the **calculations**
    - What occurs if we **divide** the *layer's* output data [1,2,3] by $2$ 

```python
softmax.forward([[0.5, 1, 1.5]])
print(softmax.output)
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210403004345911.png)

- The **output confidences now shift**
  - This due to the **nonlinearity** *nature* of the **exponentiation**
    - This is **one example** in why we **need to scale** all the **input data** to a *neural network* with the **same way**
      - Explained in chapter 22.

---

- We now add **another dense layer** as the **output layer** setting it to contain as **many inputs** as the **previous layer** and as **many outputs** as our **data** includes **classes**
  - Then apply **softmax activation** to the **output** of this *new layer*

```python
# Create dataset
X, y = spiral_data( samples = 100 , classes = 3 )

# Create Dense layer with 2 input features and 3 output values
dense1 = Layer_Dense( 2 , 3 )

# Create ReLU activation (to be used with Dense layer):
activation1 = Activation_ReLU()

# Create second Dense layer with 3 input features (as we take output
# of previous layer here) and 3 output values
dense2 = Layer_Dense( 3 , 3 )

# Create Softmax activation (to be used with Dense layer):
activation2 = Activation_Softmax()

# Make a forward pass of our training data through this layer
dense1.forward(X)

# Make a forward pass through activation function
# it takes the output of first dense layer here
activation1.forward(dense1.output)

# Make a forward pass through second Dense layer
# it takes outputs of activation function of first layer as inputs
dense2.forward(activation1.output)

# Make a forward pass through activation function
# it takes the output of second dense layer here
activation2.forward(dense2.output)
# Let's see output of the first few samples:
print (activation2.output[: 5 ])
```

- We have **completed** what we require for **forward passing** the *data* through our model
  - We use the **ReLU** activation function on the **hidden layer** which is a **per neuron basis**
- Alongside the **softmax** activation function for the **output layer** as it accepts the **non normalized values** as **input** and *outputs* the **probability distribution** which is each classes **confidence score**
  - Although neurons are **interconnected** they *each have* their own **respective** set of **weights / biases** and are **not normalized** with one another.

---

- Our example model is **random as of now**
  - To remedy this $\to$ must calculate how **wrong we are** for the **current predictions** and set to **adjust weights** and **biases** to *decrease error over time*
    - Our next step is therefore implementing a **loss function**

---



