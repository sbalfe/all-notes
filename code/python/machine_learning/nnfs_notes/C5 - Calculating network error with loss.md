# Chapter 5 - Calculating Network Error with Loss

- With random init / proper init
  - We must **train** / **teach** the model over time.
    - To **train** $\to$ we alter **weights/ biases** thus improving models **accuracy** and **confidence**
- To perform this we **calculate** how *much* **error** the *model has*
  - The **loss function** or **cost function**.
- As *loss* is the **metric** of measuring this we wish for our models loss to be **0** ideally.

---

- We **do not calculate** *error* of a model based on **argmax accuracy**
  - Earlier example of confidence:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210403005917845.png)

- If correct class was **in the centre**
  - The **model accuracy** would be **identical** between each
    - But they are **not really** *as accurate* as **one another**.
- Accuracy is **simply** applying an **argmax** to the **output** to **find the index** of the **biggest** value
  - The **output** of a **neural network** is *actually confidence*
    - And **greater confidence** means the **answer** is thus *better*.
      - We **strive** to **increase** the *correct confidence* and **decrease** the **misplaced confidence**

---

## Categorical Cross-Entropy Loss

- For **linear regression** $\to$ one of the **loss functions** used with **NN** that perform regression 
  - The **squared error** or **mean squared error** with NN

---

- We are **classifying** however therefore our loss function **differs**
  - The model uses a **softmax activation** for **output layer**
    - This is a **probability distribution**
- **Categorical cross entropy** is *explicitly* used to compare a **ground truth** *probability* (*y or "targets"*) 
  - Alongside some **predicted distribution** $\to$ y *hat* or **predictions**.
    - Therefore makes sense to deploy this technique here.
- It contains one of the **most common** *used loss functions* with a **softmax activation** on the **output layer**

---

- Formula of **categorical cross entropy** of
  1. $y$ the **desired / actual distribution**
  2. $\hat{y}$  the **predicted distribution**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210403010650955.png)

- $L_i$ $\to$ denotes the **sample loss value** 
- $i$ $\to$ is the **i-th sample** in the *set*
- $j$ $\to$ the **label / output index**
- $y$ $\to$ denotes the **target values**
- $\hat{y}$ denotes the **predicted values**

---

- Start coding solution
  - Simplify to `-log(correct_class_confidence)`
    - Formulae of this:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210727001356668.png)

- $L_i$ denotes **sample loss value**
- $i_{th}$ - the **i-th sample in the set**
- $k$ - index of the **target label** (**ground true** label)
- $y$ - denotes the **target values** and 

- $\hat{y}$ - denotes **predicted values** 

---

- Wonder why its **not called log loss** 
  - log loss error applied to **binary logistic regression**
- There is **2 classes** in this **distribution**
  - EITHER 1 / 0
- We return a **probability distribution** over all our outputs
  - Cross entropy compares **two probability distribution** 
    - For our case $\to$ have a **softmax output** $\to$ example:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210727040918800.png)

- Assume the **first item** is our **chosen item**
  - our probability distribution becomes $[1,0,0]$ 
- Cross entropy works on **probability distributions** such as $[0.2,0.5,0.3]$
  - Would **not have** to *look at one above*
    - The **desired probabilities** consist of a **1 ** within the **desired class** and **0** in the remaining classes, called **one hot**
- using this $\to$​​ the **non desired** selections are **zeroed out** and the **probability log loss** is mult by 1.
  - Making the **cross entropy calculation** easy
    - This is **==categorical cross entropy==** 

----

- Take softmax:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210727041935253.png)

- Along with **targets**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210727041955329.png)

- Apply the following;
- the idea of ln(x) here is that as the value is bounded between 1 and 0 it will also map it a value within that range but negative 
- **IT USE LOG OF E**
- the minus is basically if there is a different value other than 1,0,0 , it will first negate the number inside then switch it about to positive to view the total lost in a positive value of course.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210727042014559.png)

```python
import math
# An example output from the output layer of the neural network
softmax_output = [ 0.7 , 0.1 , 0.2 ]
# Ground truth, this is the value we wish to obtain the loss
target_output = [ 1 , 0 , 0 ]

loss = - (math.log(softmax_output[ 0 ]) * target_output[ 0 ] +
	math.log(softmax_output[ 1 ]) * target_output[ 1 ] +
	math.log(softmax_output[ 2 ]) * target_output[ 2 ])

print (loss)
>>>
0.35667494393873245
```

- Dont need to calculate indices for 0 values
  - And we dont need to mult some value by 1 obviously, so just fucking write this instead:

```python
loss = - math.log(softmax_output[ 0 ])
```

- Produces same output.
- Log here basically, if its closer to 1 > it will be closer to 0, the further away it is , just tends to infinity to indicate massive loss in our confidence value.

---

- Idea is these 2 values of [0.22, 0.6 , 0.18] and [0.32, 0.36, 0.32] both have **same argmax**
  - But the **confidence** between them is **different** as one is **more clearly the right output**. and larger,  larger = more confident , less difference.
    - The **categorical cross entropy** loss accounts for this and outputs a **large loss** for the **lower confidence** one.
- Confidence = the **larger value argmax choice** , i.e 0.9 in 0.05 / 0.05 over 0.4 in 0.3 and 0.3

```python
import math
print (math.log( 1. ))
print (math.log( 0.95 ))
print (math.log( 0.9 ))
print (math.log( 0.8 ))
print ( '...' )
print (math.log( 0.2 ))
print (math.log( 0.1 ))
print (math.log( 0.05 ))
print (math.log( 0.01 ))
>>>
0.0
- 0.05129329438755058
- 0.10536051565782628
- 0.2231435513142097
...
- 1.6094379124341003
- 2.3025850929940455
- 2.995732273553991
- 4.605170185988091

# closer to 1  = 0
```

- Update our model to work on **batches** of **softmax output distributions**
  - Secondly $\to$ make the **negative log** calculation **dynamic** to the **target index**
    - The *target index* so far has been **hard coded**
- Consider 3 batches for 3 classes:

```python
# Probabilities for 3 samples
softmax_outputs = np.array([[ 0.7 , 0.1 , 0.2 ],
[ 0.1 , 0.5 , 0.4 ],
[ 0.02 , 0.9 , 0.08 ]])
```

- Must **dynamically calculate** the *categorical cross entropy*
  - We know is a **negative log calculation**
    - Determine which value in the **softmax output** to *calculate* the **negative log form**
      - just need to know our **target values**. 

---

- **3 classes** for this
  - example $\to$ trying to **classify** a *dog / cat / human* : 0 / 1 / 2 
    - Assume the **batch** of **three samples** inputs to the **neural network** are **mapped** to the **target values** of a *dog , cat and cat*
      - Therefore targets list of indices becomes $[0,1,1]$

```python
softmax_outputs = [[ 0.7 , 0.1 , 0.2 ],
				   [ 0.1 , 0.5 , 0.4 ],
				   [ 0.02 , 0.9 , 0.08 ]]

class_targets = [ 0 , 1 , 1 ] # dog, cat, cat
```

- 0 > means that this **sample** provided was intended to be target at the **dog index**
- 1 > this sample provides a **0.5** **confidence** rating that it was a **cat**. 
- The model is **less certain** therefore for the cat, however last one for cat **is 0.9** which is **extremely confident**

---

- **Map** the **indices** *to retrieve values* from **softmax distributions**

```c++
softmax_outputs = [[ 0.7 , 0.1 , 0.2 ],
					[ 0.1 , 0.5 , 0.4 ],
					[ 0.02 , 0.9 , 0.08 ]]
class_targets = [ 0 , 1 , 1 ]
    
for targ_idx, distribution in zip (class_targets, softmax_outputs):
	print (distribution[targ_idx])
>>>
0.7
0.5
0.9
```

- Simplified into a **numpy array**

```python
softmax_outputs = np.array([[ 0.7 , 0.1 , 0.2 ],
							[ 0.1 , 0.5 , 0.4 ],
							[ 0.02 , 0.9 , 0.08 ]])

class_targets = [ 0 , 1 , 1 ]

print (softmax_outputs[[ 0 , 1 , 2 ], class_targets])
>>>
[ 0.7 0.5 0.9 ]
```

- $[0,1,2]$ values are **numpy** allowing us to **index an array differently**
  - We index it based on the **indices** that **class targets** provides us.
- Issue $\to$ this must **filter data rows** within the **array** $\to$ the **second dimension**
  - Perform this $\to$​ must **explicitly filter** this array in the **first dimension**
    - This **dim** contains the **predictions** therefore must **retain them all**
- We use them all therefore call **range** to produce a **series** of numbers in the **length of the softmax outputs** which is 3 $\to$ does not include last item as it goes from **0 to 2** like we want 

```python
print (softmax_outputs[
range ( len (softmax_outputs)), class_target ])
>>>
[ 0.7 0.5 0.9 ]
```

- Returns a **list of confidences** at the **target indices** for *each of the samples*
  - Apply the **negative log** to *this list*

```c++
print ( - np.log(softmax_outputs[
range ( len (softmax_outputs)), class_targets]))
>>>
[ 0.35667494 0.69314718 0.10536052 ]
```

- Must calculate an **average loss** for **each batch** to have an **idea** on the model **accuracy** during **training**. 
  - Numpy has a method that **computes** averages on **arrays**

```python
# range (softmax outputs length) returns an array of indices to be used by numpy to target the correct
 neg_log = - np.log(softmax_outputs[range ( len (softmax_outputs)), class_targets])
average_loss = np.mean(neg_log)
print (average_loss)
>>>
0.38506088005216804
```

- Targets may be
  1. one hot encoded (1 value right all others wrong)
  2. sparse $\to$ the number they contain are the **correct class numbers**
     - Generated this way in the **spiral_data()** function
       - Can **enable** the **loss calculation** to *accept either form*.
       - Add a **check** to see if they are **one hot encoded** 
- We add a check via **counting dimensions**
  - If its **list 1D** $\to$ **sparse**
  - if its **2D** $\to$ a *set* of **one hot encoded vectors**
- Implement solution using **first equation** of *this chapter*
  - Rather than **filtering** out the **confidences** at the *target label*
    - We **multiply confidences** by the **targets** $\to$ *zeroing* out **all values** apart from the **ones at correct labels**.
      - Performing a **sum** along the **row axis 1**
- Add a **test code** to **the code** we just wrote for the **number of dimensions**
  - Also **move calculations** of **log values** to *out* of the **if statement**
    - implement the **solution** for **one hot encoded labels** following **first equation**

```python
import numpy as np
softmax_outputs = np.array([[ 0.7 , 0.1 , 0.2 ],
                            [ 0.1 , 0.5 , 0.4 ],
                            [ 0.02 , 0.9 , 0.08 ]])
class_targets = np.array([[ 1 , 0 , 0 ],
                        [ 0 , 1 , 0 ],
                        [ 0 , 1 , 0 ]])
# Probabilities for target values -
# only if categorical labels
if len (class_targets.shape) == 1 :
    # just a list therefore just access the values from index of targets.
	correct_confidences = softmax_outputs[range ( len (softmax_outputs)),class_targets]

# Mask values - only for one-hot encoded labels
elif len (class_targets.shape) == 2 :
    # summate the values multiplying the softmax output by class target which is a sort of binary AND selection
    correct_confidences = np.sum(softmax_outputs * class_targets, axis = 1)
    
# Losses
neg_log = - np.log(correct_confidences)
average_loss = np.mean(neg_log)
print (average_loss)
```

- **Issue now**
  - The **softmax output** $\to$ which is **input** for this **loss function**
    - Consists of **numbers** in the **range** of **0 to -1** *list of confidences*
      - Its possible that the **model** has **full confidence** for *some label* , meaning the others are **zero**.

- Possible to assign **full confidence** to something that was **not the target**
  - Trying to calculate **loss** results in **log zeroing error**

```python
## this is useful result as e^(- infinity) produces a result of 0.
np.e ** ( - np.inf)

>>>
0.0 # the limit of this function is 0
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210727055345738.png)

- -np.log(0) makes sense to be **-infinity** but it would be **incorrect** on **optimization calculations**
  - Later with **optimizations** $\to$ have a **problem** calculating the **gradients**
    - Starting with a **mean value** of *all sample wise losses* since a **single infinite value** in the list causes the **average** of the list to be infinite

```python
import numpy as np
np.mean([ 1 , 2 , 3 , - np.log( 0 )])
>>>
__main__: 1 : RuntimeWarning : divide by zero encountered in log
inf
```

- Possibly add a **very small number**

```python
We could add a very small value to the confidence to prevent it from being a zero, for example,
1e-7 :
- np.log( 1e-7 )
>>>
16.11809565095832
```

- This affects the result **insignificantly** 
  - Yields **additional issues**
    - A confidence value of **1**:

```python
- np.log( 1 + 1e-7 )
>>>
- 9.999999505838704e-08
```

- Placing all **confidence** in correct label should become **0** loss $\to$ the goal of **neural networks**. 

- Rather than **adding it on** just **clip it off from 1**
  - To make it **slightly less than fully confident**

```python
- np.log( 1 - 1e-7 )
>>>
1.0000000494736474e-07
```

- Use **np.clip()** to help this 

```python
y_pred_clipped = np.clip(y_pred, 1e-7 , 1 - 1e-7 )
```

- Method performs **clipping** on an **array of values**
  - Therefore we can **apply** to the **predictions directly** and *save as a seperate array*
    - Which we will **use shortly.**




```python
import numpy as np
import nnfs
from nnfs.datasets import spiral_data

nnfs.init()

class Layer_Dense:
    def __init__(self, n_inputs, n_neurons):
        # init the weights and biases
        
        # set the weights
        # https://www.mathsisfun.com/data/standard-normal-distribution.html, default mean = 0 and variance =1 
        # this just creates a random from -3 to 3 , but multiplies it by 0.01 to gain a faster time to calculate
        # NN prefer data between 1 and -1
        # https://numpy.org/doc/stable/reference/random/generated/numpy.random.randn.html
        self.weights = 0.01 * np.random.randn(n_inputs, n_neurons)
        
        # create an array of shape (1, n_neurons) f
		self.biases = np.zeros(( 1 , n_neurons))
    def forward(self, inputs):
        self.output = np.dot(inputs, self.weights) + self.biases

class Activation_ReLU:
    
    #forward pass
    def forward(self, inputs):
        #calculate output values from inputs
        self.output = np.maximum(0, inputs)
        
        
class Activation_Softmax:
    #forward pass
    
    def forward(self, inputs):
        
        # get unnormalized probabilities
        exp_values = np.exp(inputs - np.max(inputs, axis = 1, keepdims = True))
        
        # normalize them for each sample
        probabilties = exp_values / np.sum(exp_values, axis = 1, keepdims = True)
        
        self.output = probabilities
        
# creates the input data, we obtain 2 features from this to create 100 samples of 3 classes       
X, y = spiral_data(samples = 100, classes = 3)

# 2 input features and 3 output values
dense1 = Layer_Dense(2,3)

# create relu activation to be used in dense layer
activation1 = Activation_ReLU()

# Create second Dense layer with 3 input features (as we take output
# of previous layer here) and 3 output values (output values)
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

print (activation2.output[: 5 ])
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210726184654651.png)

### Accuracy Calculation

- Loss is **useful metric** for **optimizing model**
  - The metric is **commonly** used in *practice* along with **loss is the accuracy**.

- This describes how **often** the **largest confidence** is the **correct class** in terms of a *fraction*
  - Can **reuse** *existing variables* definition to **calculate** this **accuracy metric**
    - Use the **arg max values** from **softmax outputs** then *compare* to the **targets**

---

- This is simply as doing

```python
import numpy as np
# Probabilities of 3 samples
softmax_outputs = np.array([[ 0.7 , 0.2 , 0.1 ],
					[ 0.5 , 0.1 , 0.4 ],
					[ 0.02 , 0.9 , 0.08 ]])

# Target (ground-truth) labels for 3 samples
class_targets = np.array([ 0 , 1 , 1 ])

# Calculate values along second axis (axis of index 1)
predictions = np.argmax(softmax_outputs, axis = 1 )

# If targets are one-hot encoded - convert them
if len (class_targets.shape) == 2 :
	class_targets = np.argmax(class_targets, axis = 1 )

# True evaluates to 1; False to 0
accuracy = np.mean(predictions == class_targets)
print ( 'acc:' , accuracy)
>>>
acc: 0.6666666666666666
```

- Also handling **one hot encoded targets** by conversion to **sparse values** using **np.argmax()**
