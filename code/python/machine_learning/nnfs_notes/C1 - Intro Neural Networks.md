# Introducing neural networks

- Also called **artificial neural networks**.
- A *type* of **machine learning** $\to$ conflated with *deep learning*.

---

- Defining characteristic $\to$ having **two or more** *hidden layers*
  - The NN controls these layers.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326110701347.png)

---

## What is a neural network

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326111029140.png)

- Inspired by the **organic brain**
  - There are **neurons** / **activations** and *lots* of *interconnectivity* 
    - Even when the processes underneath are **vastly different**. 

- Many neurons together $\to$ produce *relationships* that frequently **outperform** many other *machine learning methods*.

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326111256416.png)

---

- NN $\to$ **black boxes**
  - we do not **have no idea** how they *reach conclusions* however we *understand* how they do this.

- **Dense layers** $\to$ the *most common* layer
  - Consist of **interconnected neurons** $\to$ **every neuron** of the *given layer* $\to$ connected to **every neuron** of the *next layer*.
    - The **output** value becomes **input** for the *next neuron*. 
- Every **connection** between the *neurons* has some **weight** associated with it.
  - this is a **trainable factor** of *how much* of the **input** to actually use. 
    - The **weight** becomes **multiplied** by the *input value*. 
- Once every **input-weights** $\to$ flow into the neuron
  - They are **summed** and a **bias** (*another trainable parameter*) $\to$ added. 
    - The **bias** is to *offset* the **output** in some **positive / negative** way. 

---

- The *concept* of **weights/biases** $\to$ '*knobs*' 
  - You can **tune** to *fit* the model to **data**. 
- In a **neural network** $\to$ there are often **thousands** or *even millions* of **these parameters** that are *tuned* by the **optimizer** in **training**.
  - Why not just have *only bias* or *only weights* 
    - These are both **tuneable parameters** $\to$ therefore both **impact the neurons'** *output* 
      - Achieve this in **different ways**.
- **Weights** $\to$ are *multiplied* $\to$ only change the **magnitude** or *flip* the sign from **positive** to **negative**
  - $Output = weight\cdot{input}+bias$
    - Is **not unlike** the *equation* for a line $y=mx+b$ $\to$ visualise this:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326114752222.png)

- *ADJUSTING* the **weight** impacts the **slope** of the **function**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326114934855.png)

- As you **increase** the *value of the weight* $\to$ obtain a **steeper slope**.

- As you **decrease** the *value of the weight* $\to$  the slope **turns negative**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326115209395.png)

- Give **idea** on how **weights** can **impact** the *neurons* **output** value that you obtain from $input\cdot{weights}+bias$ 

---

- For the **bias parameter** $\to$ the *bias* will **offset** the *overall function*
  - With weight 1 and bias 2:



![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326115452521.png)

- As you **increase** the *bias* $\to$ the **overall function** will *shift upwards*.

- As you **decrease** the *bias* $\to$ the **overall function** will *move downwards*.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326115717475.png)

---

- The **weights** and **biases** help *impact* the **output** of **neurons**
  - They achieve this in **different ways** as seen. 
    - Make clear in **activation functions later**
- As a **general overview** $\to$ the *step function* to **mimic a neuron** in the *brain* 
  - That is either *'firing'* or *not* like **on/off**
- In **programming** $\to$ the on/off switch as a **function** is the **step function** as it looks like a **step** when you **graph it**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326120103859.png)

- For a *step function* $\to$ if the **neurons** *output value* 
  - Calculated by $sum(inputs\cdot{weights})+bias$ is **greater than 0** 
    -  The neuron will **fire** on *output = 1*. 
    - Otherwise does **not fire** on *output = 0*. 

- Formulae for a single neuron

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326120539596.png)

- Then **usually apply** an *activation function* to this output
  - Noted via **activation()**. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326120615779.png)

- You can use a **step function** for the *activation function*
  - You can use **more advanced techniques**.
- NN today $\to$ use more **informative activation functions** such as the **Rectified linear** *ReLU* activation function.
  - Every **neurons output** $\to$ may be *part* of the **ending output layer**
    - Alongside being the **input** to *another layer of neurons*. 
- The **full function** of a NN may be *larger* $\to$ show a **simple** example with **2 hidden layers** of **4 neurons each**.

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326120945981.png)

---

- Alongside 2 **hidden layers** $\to$ there is the **input / output** layers

---

- **Input layer** $\to$ is the **actual input data** 
  - May be **pixel values** from an **image** or **data** from a **temperature sensor**.

- Data can be ***raw*** in the **collected form**
  - Often some **pre-process** is applied from functions such as **normalization** / **scaling**
- Inputs must be **numerical**. 
- Have the values range from $0$ to $1$ or $-1$ and $1$ is common in  **pre-processing**.
  - Either use both *scaling/normalization functions*

---

- **Output Layer** $\to$ the **return** of the *neural network*
  - In **classification** $\to$ you aim to **predict** the *class* of the **input**
    - The **output layer** often contains as **many neurons** as the *training dataset* has *classes*. 
    - May have a **single output** neuron for **binary** *classification* 
![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326122604390.png)

---

- For **every image** passed in the **neural network** $\to$ the *final output* has some **calculated value** in the *'cat'* output neuron and 'dog' respectively. 
- The **output neuron** with the **greatest score** $\to$ becomes the **class prediction** $\to$ for the *image* used as **input**. 

---

- Journey of **what is going on** during a simple **forward pass of data**.
  - NN $\to$ a **bunch** of **math equations** that programmers turn to **code**. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326123545610.png)

- Math behind a **forward pass** of **example neural network model**

---

- This can be coded in numpy also but we will **break it down** into the **components**. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210326123725575.png)

- Separating each **mathematical** section into its components makes this understandable. 

- With **millions** of variables involved $\to$ related to **neurons** arranged as **interconnected layers** 
  - There must exist some **combination** of *values* for these variables that yield the **desired output**. 
    - Finding the **combination** of *parameters* (*weights* and *bias*) values is the **challenging part**. 

---

- END GOAL $\to$ to **adjust** the **weights / biases** 
  - So when applied to **unseen** example in input $\to$ produce the **desired output**. 
- Supervised learning algorithms are **trained** $\to$ show **examples** of *inputs* / *outputs*
  - An issue is **overfitting** $\to$ only learns to **fit** the **training data** but has *no understanding* of the **underlying** *input/output* dependencies. 
    - Essentially just **memorizes** the **training data**. 

---

- We therefore use ***in sample*** data to *train the model*.
  - Then use ***out of sample*** data to *validate the algorithm*. 
- Set **percentages** are *set aside* for **both datasets** to *partition the data*
  - Example $\to$ there is a **dataset** of $100,000$ samples of **data/labels**
    - immediately take $10,000$ and **set aside** to be **out of sample** or *validation data*.
      - Then **train** the model with the $90,000$ **in sample** or *training data* then *validate* with the $10,000$.
- Goal $\to$ accurate prediction on the **training data**
  - But also **similarly accurate** when predicting on the **withheld** *out of sample* *validation data*

---

- This is referred to as **generalization**. 
  - This means learning to **fit** the *data* rather than **memorizing** it.
    - Idea is to **train** via weight adjustment over time on **many examples of data**. 
      - Then take **out of sample data** $\to$ that the network has **never seen** and **hope** it predicts well. 

---

- The training is based on **how wrong the NN are**
  - To calculate the *error* $\to$ the **loss** and *slowly* attempt to **adjust the parameters**
    - Over **many iterations** $\to$ the network **gradually** becomes **less wrong**
- Goal of NN $\to$ **generalization**
  - The network can **see many examples** of *never before seen data* and *accurately output* the **values** that you **hope to achieve.**
- Other use cases $\to$ regression (predict scalar, singular, value) , clustering (unsupervised structure into groups), etc.
-  Classification $\to$ the most common.

---

- Features $\to$ an array of data used as the input to the neural network for classification.

