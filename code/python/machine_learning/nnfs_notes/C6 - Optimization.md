

[TOC]



# Optimization

- Must determine **how to adjust** each **weight / bias** to **decrease loss**

---

- Basic option is **randomly altering**  > **loss check** > repeat until at a **suitable loss**
- Example:

```python
import matplotlib.pyplot as plt
import nnfs
from nnfs.datasets import vertical_data
nnfs.init()

X, y = vertical_data( samples = 100 , classes = 3 )

# X[:, 0] > select all the values from only the first index
# X[:, 1] > select all  the values from only the second index

# https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.scatter.html
# c basically decides the class or colour basically
plt.scatter(X[:, 0 ], X[:, 1 ], c = y, s = 40 , cmap = 'brg' )
plt.show()
```

###### https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.scatter.html

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210812142530379.png)



- Using previous code
  - Use the new dataset with a simple neural network

```python
# Create dataset
X, y = vertical_data( samples = 100 , classes = 3 )

# Create model
dense1 = Layer_Dense( 2 , 3 ) # first dense layer, 2 inputs

activation1 = Activation_ReLU()

dense2 = Layer_Dense( 3 , 3 ) # second dense layer, 3 inputs, 3 outputs

activation2 = Activation_Softmax()

# Create loss function
loss_function = Loss_CategoricalCrossentropy()
```

- Create variable to **track best loss**.
  - Alongside the **associated** weights and **biases**.

```python
# Helper variables
lowest_loss = 9999999 # some initial value
best_dense1_weights = dense1.weights.copy()
best_dense1_biases = dense1.biases.copy()
best_dense2_weights = dense2.weights.copy()
best_dense2_biases = dense2.biases.copy()
```

- Overwrite the **loss** when a **smaller** loss is found
- Also **copy** the **weights and biases** of both layers.
- Then **iterate** over the **required times**
  - Pick some **random** values for **weights** and **biases**
    - Then **save** the **weights biases** $\to$ if they generate the **lowest seen loss**

```python
for iteration in range ( 10000 ):
    
    # Generate a new set of weights for iteration
    dense1.weights = 0.05 * np.random.randn( 2 , 3 )
    dense1.biases = 0.05 * np.random.randn( 1 , 3 )
    dense2.weights = 0.05 * np.random.randn( 3 , 3 )
    dense2.biases = 0.05 * np.random.randn( 1 , 3 )

    # Perform a forward pass of the training data through this layer
    dense1.forward(X)
    activation1.forward(dense1.output)
    dense2.forward(activation1.output)
    activation2.forward(dense2.output)

    # Perform a forward pass through activation function
    # it takes the output of second dense layer here and returns loss
    loss = loss_function.calculate(activation2.output, y)

    # Calculate accuracy from output of activation2 and targets
    # calculate values along first axis
    predictions = np.argmax(activation2.output, axis = 1 )
    accuracy = np.mean(predictions == y)

    # If loss is smaller - print and save weights and biases aside
    if loss < lowest_loss:
    print ( 'New set of weights found, iteration:' , iteration,
    'loss:' , loss, 'acc:' , accuracy)
    best_dense1_weights = dense1.weights.copy()
    best_dense1_biases = dense1.biases.copy()
    best_dense2_weights = dense2.weights.copy()
    best_dense2_biases = dense2.biases.copy()
    lowest_loss = loss
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210812195303664.png)

- Loss gradually falls, **inclemently however**
- The overall **accuracy** does **not improve** 
  - Apart from a **random case** of course of **lucky weight/bias draw**.
- With a **massive loss**
  - This state is **not stable**
- Running further **90k iterations** for **100k** in total.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210812195557300.png)

- Loss continues to drop
  - But accuracy **did not change**
    - This is not reliable at all for **loss minimizing**
- After **1 billion iterations** $\to$ this was the **best result**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210812201550277.png)

- Rather than **setting parameters** with **randomly values** 
  - Apply a **fraction** of the **values** to **parameters**.
    - Weights updated from what **currently yields** the **lowest loss**
- Rather than **aimlessly randomly**.
  - If the **adjustment** ends in **decreasing**.
    - make some **new point** of **adjustment**
  - If **increases**
    - *revert* to **previous point**
- First change from **random selection** to *randomly adjusting* 

```python
# Update weights with some small random values
dense1.weights += 0.05 * np.random.randn( 2 , 3 )
dense1.biases += 0.05 * np.random.randn( 1 , 3 )
dense2.weights += 0.05 * np.random.randn( 3 , 3 )
dense2.biases += 0.05 * np.random.randn( 1 , 3 )
```

- Change the ending if:

```python
# If loss is smaller - print and save weights and biases aside
if loss < lowest_loss:
print ( 'New set of weights found, iteration:' , iteration,
'loss:' , loss, 'acc:' , accuracy)
best_dense1_weights = dense1.weights.copy()
best_dense1_biases = dense1.biases.copy()
best_dense2_weights = dense2.weights.copy()
best_dense2_biases = dense2.biases.copy()
lowest_loss = loss
# Revert weights and biases
else :
dense1.weights = best_dense1_weights.copy()
dense1.biases = best_dense1_biases.copy()
dense2.weights = best_dense2_weights.copy()
dense2.biases = best_dense2_biases.copy()
```

