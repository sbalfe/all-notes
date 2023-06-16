# layers

- There is just **one layer** so.
  - To make it **deep** $\to$ must have **2 or more hidden layers**.
    - At the **moment** $\to$ there is just an **output layer**.

- We want many **hidden layers** $\to$ see later.
  - A **hidden layer** $\to$ any layer *not an input* or *input*.
    - They contain **values** that we *do not deal with it* $\to$ normally. 
      - hence *hidden*
- The values can be **used** to *diagnose* the **neural network**. 
  - Explore this concept $\to$ add **another layer** to **this neural network** 
    - For *now* $\to$ assume these **two layers** are the **hidden layers** and we have **coded** the **output layer** yet.

- **Before adding** $\to$ for the **first layer** $\to$ we can see we have some **input** with **4 features**.

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210327040522902.png)

---

- **Samples** ( *feature set data* ) $\to$ are **fed** through the **input**
  - This does **not change** in **any way** 
    - To the **first** *hidden layer* $\to$ which we see **has** *3 sets of weights* with **4 values each**
- Each of the **3 unique weights** $\to$ is *associated* with its **distinct neuron**
  - Therefore there are **3 weight sets** there are **3 neurons** in the *first hidden layer*
- Each **neuron** contains a *unique* set of **weights**.
  - Which we have **4** of as there are **4 inputs**
    - This is why our inital weights are **shaped** as $(3,4)$ 

---

- To **add another layer** $\to$ Ensure the **expected input** to the **layer** will **match** the *previous layer' output*
  - We **set** the **number** of **neurons** in a *layer* via setting how many **weight sets** and **biases** there are.
    - The *previous layer'* influence on **weight sets** for the **current layer** is that *each weight* set requires a **seperate** *weight* per **input**.
      - Requires a **distinct** *weight* per neuron from **previous layer** (*feature* if we are talking on **input layer**)

- The **previous layer** $\to$ has **3 weight sets** and **3 biases** $\to$ therefore we know its **3 neurons**. 
  - For the **next layer** we can **choose** as **many weight sets** as we wish
    - This decides the **number of neurons** for the **next layer**. 
      - Each of these sets must have **3 discrete weights**.
- To **create** some **new layer** $\to$ *copy / paste* our **weights** and **biases** to **weights2 / biases2**
  - Change the *values* to **new made up sets**:

```python
inputs = [[1, 2, 3, 2.5],
[2., 5., -1., 2],
[-1.5, 2.7, 3.3, -0.8]]

weights = [[0.2, 0.8, -0.5, 1],
[0.5, -0.91, 0.26, -0.5],
[-0.26, -0.27, 0.17, 0.87]]
biases = [2, 3, 0.5]

weights2 = [[0.1, -0.14, 0.5],
[-0.5, 0.12, -0.33],
[-0.44, 0.73, -0.13]]
biases2 = [-1, 2, -0.5]

layer1_outputs = np.dot(inputs, np.array(weights).T) + biases
layer2_outputs = np.dot(layer1_outputs, np.array(weights2).T) + biases2
print(layer2_outputs)
```

- As **previously stated**
  - The *inputs* to **layers** are *either inputs* from the **actual dataset** you **train with** our *outputs* from the **previous layer**
    - Thats why there are **2 versions** of **weights / biases** but just a **single input** 
      - Input for hidden layer 2 is output of layer 1. 

- Visually represent the network  as this

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210327042732762.png)

- **4 features** input into **2 hidden layers** of **3 neurons each**.

## Training data

- Rather than **hand typing** the **random data**
  - Use a **function** to create **non linear data**
    - This is data that can **not be represented** with a **straight line**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210327043017700.png)

- Orange dots that can be **represented** (*fit*) by a **straight green line**

---

- However for **non linear data**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210327043115832.png)

---

- If you **grouped** the *data points* of form $(x,y)$ where $y=f(x)$ 
  - And it appears to be a **line** with some **clear trend/slope** $\to$ most likely some **linear data**.
    - These types of data is **easily approximated** by **far simple** *machine learning models* than *neural networks*

---

```python
from nnfs.datasets import spiral_data
import nnfs

# sets random seed to 0 as default, creates a float32 dtype default, overrides the dot product of numpy
# ensure repeatable results for the following along.

nnfs.init()
'''
 The spiral_data function is enables us to create a dataset with as many classes as we wish
 The function has parameters to choose the number of classes and number of
 points/observations per class in the resulting non liner dataset
 
'''
```

----

```python
import matplotlib.pyplot as plt
X, y = spiral_data(samples=100, classes=3)
plt.scatter(X[:,0], X[:,1], c = y, cmap = 'brg')
plt.show()

```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210327043751189.png)

- If you **trace from center** $\to$ can determine all **3 classes separately** 
  - This is a **very challenging problem** for a *learning classifier to solve*
    - Adding **color makes** this **clearer**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210327043942438.png)

- The **NN** is *not aware* of the colour of the data
  - As this data is **not encoded**
- Each **dot** is the **feature** and the **coordinates** are **samples** that *form* the dataset
  - The **classification** for that dot is **concerned** with which **spiral** its a part of it **RGB** 
    - The colours are then assigned a **class number** for the **model to fit**
      - 0,1,2 for example. 

## Dense layer class

- We do not require any **hand typed** *data now*
  - Create something **similar** for our **various types** of *neural network layers*
- We have created only **dense** / **fully connected** *layers*
  - Called *dense* layers in PAPERS / LITERATURE / CODE
    - Alt $\to$ **fully connected**. 
- **Dense layer** *class* begins with **two methods**.

---

```python
class Layer_Dense:
	def __init__(self, n_inputs, n_neurons):
        #initialize weights and biases
        pass # placeholder
    
    #Forward pass
    def forward(self, inputs):
        # calculate output values from inputs, weights and biases
        pass 
```

- Weights are **often initialized** *randomly* for a **model**
  - **NOT ALWAYS**
- If you want to **load** some *pre-trained model*
  - You **initialize** the **parameters** to **whatever** the selected *pretrained* *model* **finished** with.
- Possible $\to$ some **new model** $\to$ have some *other initialization rules* **besides random**. 
  - For now $\to$ we **keep random**. 

---

- We have the **forward** method 
  - When we **pass data** through some **model** from *beginning* to *end* 
    - This is a **forward pass**
- This is **not the only method**
  - Can perform the **data loop back** around and **do other interesting things**
    - Standard for now with **regular forward pass**.

---

```python
class Layer_Dense:
    # Layer initialization
	def __init__(self, n_inputs, n_neurons):
        #initialize weights and biases
        
        self.weights = 0.01 * np.random.randn(n_inputs, n_neurons)
        self.biases = np.zeros((1, n_neurons))
    
    #Forward pass
    def forward(self, inputs):
        # calculate output values from inputs, weights and biases
        pass 
```

- We set weights to be **random** and **biases** to be $0$.
  - Note init weights to $(inputs , neurons)$ rather than alt
    - This is **ahead** of **transposing** every time we **perform** the *forward pass*.

- Zero bias $\to$ many samples containing values of 0
  - A **bias** can **ensure** a neuron  **fires initially**
    - May be **appropriate** to **init** the *biases* to a **non zero number**
      - Most *common* $\to$ init for bias is 0.
    - Vary on **use case** $\to$ one of the **many things** that requires **tweaking** when attempting to **improve results**.

- Example of **trying something else** $\to$ **dead neurons**
  - Have **not** gone over **activation functions** but for **step function** again:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210329024620539.png)

- Possible for $weights\cdot{inputs}+biases$ to **not meet threshold** for this **step function**
  - Neuron **outputs** $0$. 
    - Not **big issue** by itself
      - Becomes a **problem** if it occurs for **every** one of the **input samples**. 
        - Clear when we **reach backpropagation**.

- This $0$ *inputs* to the **next neuron**
  - **Any weight** will thus **be multiplied** by **zero** and *become zero*.
    - With an **increasing** *number of neurons* producing zero output $\to$ **more inputs** receive the 0 essentially **rendering** it a **non-trainable** network or **dead**

---

- Use $np.random.randn$ and $np.zeros$ in **more detail**
  - These methods are used to **initialize arrays**
- $np.random.randn$ $\to$ produces a **gaussian distribution** (*normal*). 
  - Mean of $0$ and **variance** $1$ 
    - It will **generate** *random* numbers , positive and negative **centred** at $0$ as the **mean** is *close to 0*

- **Neural networks** $\to$ work **best** with values in the range $-1\leq{x}\leq{1}$ $\to$ discuss later.
  - This function thus **generates** these **random values** for these numbers.

- **Multiply** this **gaussian distribution** for the *weights* by $0.01$ to **generate numbers** that are **couple magnitudes** *smaller*
  - Otherwise $\to$ **time** to *fit* the data during **training process** as *starting values* will be **disproportionately** *large compared* to the **updates** being made **during training**
- **Overall idea** $\to$ start with **non zero values** that are *small enough* to **fuck up training**.
  - There are a **bunch** of **values** to *begin working* with but **hopefully** that are **not too large** or **zeros**.
    - Can **experiment** with **values** other than $0.01$ if **you like**.

---

- the function takes **dimension sizes** as **parameters** and *creates* the **array** with **this shape**
  - The **weights** will **be the number** of **inputs** for the **first dimension** and the **number of neurons** for the **2nd dimension**
- *Similar* to to **our previous** $\to$ made up **array** of **weights** $\to$ *just randomly generated*
  - When there is a **function** or **block of code** that you are **not certain** $\to$ just **print it out**

```python
import numpy as np
import nnfs
nnfs.init()
print(np.random.randn(2,5))
```

- This returns a $2x5$ array $\to$ **shape** of $(2,5)$ 
  - Data **randomly** *sampled* from a **gaussian distribution** with a *mean* of $0$. 

---

- Next $\to$ the $np.zeros$ takes a **desired array shape** as an *argument* and **returns an array** that *shape* *filled* with **zeros**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210329032501870.png)

- **init** the *bias* with shape of $(1,\text{n neurons})$​ as a **row vector**
  - Let us **easily** *add* to the **result** of **dot product later** with **no additional operations** of *transposition*.

```python
n_inputs = 2
n_neurons = 4
weights = 0.01 * np.random.randn(n_inputs, n_neurons)
biases = np.zeros(( 1 , n_neurons))
print (weights)
print (biases)
```

- On to **the forward method** $\to$ must update with dot product and bias calculation.

```python
class Layer_Dense:
    # Layer initialization
	def __init__(self, n_inputs, n_neurons):
        #initialize weights and biases
        
        self.weights = 0.01 * np.random.randn(n_inputs, n_neurons)
        self.biases = np.zeros((1, n_neurons))
    
    #Forward pass
    def forward(self, inputs):
        # calculate output values from inputs, weights and biases
        self.output = np.dot(inputs, self.weights) + self.biases
```

- We are **ready** to use **the new class** instead of **hardcode calculations**
  - Generate **some data** using the **discussed dataset** creation method
    - use **new layer** to perform a **method pass**. 

```python
# create a dataset
X,y = spiral_data(samples = 100, classes = 3)

# Create Dense Layer with 2 input features and 3 output values
dense1 = Layer_Dense(2,3)

#Peform a forward pass of our training data through the layer
dense1.forward(X)

#ouput of first few sampless

print(dense1.output[:5])
```

- In the **output** $\to$ there are **5 rows** of **data** with **3 values for each**
  - Each of these **3 values** is the **value**  from the **3 neurons** within the **dense1** layer after **passing** *each sample*
    - There is thus a **network of neurons**
      - Nearly the most basic NN but misses **activation functions**.

