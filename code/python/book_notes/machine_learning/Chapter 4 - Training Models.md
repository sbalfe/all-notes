# Training models

## Linear Regression

- Looked at **simple regression model earlier** $\to$ $\text{life satisfaction} = $ $\theta_0 + \theta_1  \times \text{GDP - per - capita}$
  - This is a **linear function** of the *input feature* `GDP_per_capita` with $\theta_0$ and $\theta_1$ as the **model parameters**.

---

- A *linear model* that makes it **prediction** by *simply* computing a **weighted sum of the input features**, along a **constant term** called the **bias term** (*intercept term*)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113161303998.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113161319691.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113161328334.png)

- Written more **concisely** using some **vectorized form**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113161418472.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113161427530.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113161439011.png)

- Training a model means **setting the parameters**
  - So the **model best fits** the **training set**
    - For this **purpose** $\to$ first *need a measure* of **how well** (*poorly*) the **model fits** the **training data**. 
- In chapter 2 $\to$ saw a **common performance** of a *regression model* is the **RMSE** (*root mean squared error*)
  - Thus to **train a linear regression model**
    - Must find the **value** of $\theta$ that **minimizes** the **RMSE**
      - Therefore to **train a linear regression model** $\to$ you *need to find value* of $\theta$ that *minimizes* the **RMSE**
- In *practice* $\to$ its simpler to **minimize** the *mean square error* $MSE$ from $RMSE$ 
  - This leads to **same results** (*because* the value that **minimizes** a *function* also **minimizes** its **square root**)

---

- The $MSE$ of a **Linear regression hypothesis** $h_{\theta}$ on a training set $X$ is **calculated** using this **equation** :

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113162722659.png)

- Most of **these notations** were *presented* in **chapter 2**
  - The only **difference** is that we write $h_{\theta}$ instead of just $h$ in **order** to **make it clear** that **model** is **parametrized** by the *vector* $\theta$ 
    - To **simplify notations** , just write :

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113163350349.png)

### Normal equation

- Finding the **value** of $\theta$ that **minimizes** the *cost function*
  - There is **a closed form solution** in *other words* $\to$ a *mathematical equation* that **gives** the **result directly**
    - This is the **normal equation**.

https://www.youtube.com/watch?v=1kkVEcmhkL8 - derivation

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113163613761.png)

- generate some **linear looking data** to *test this equation* 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113163737433.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113163815696.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113163829926.png)

- Now **lets** *compute* $\hat{\theta}$ using **normal equation**
  - Using the `inv()` function from **linear numpy** `np.linalg` to **compute the inverse of matrix**
    - And the `dot()` method for **matrix multiplication**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113164340032.png)

- The *actual function* that we **used to generate** the data $y= 4+3x_1 + \text{gaussian noise}$ 
  - See what **the equation found**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113164526859.png)

- We *would* have **hoped** for $\theta_0 = 4$ and $\theta_1 = 3$ rather than $\theta_0 = 4.215$  and $\theta_1 = 2.770$ 
  - Close **enough** $\to$ the **noise** *made* it **impossible** to *recover* the **exact parameters** of the **original function**

- Make **predictions** using $\hat{\theta}$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113165229139.png)

- Plot this **model prediction.** 

```python
plt.plot(X_new, y_predict, "r-")
plt.plot(X, y, "b.")

plt.axis([0, 2, 0, 15])
plt.show()
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113173333586.png)

- Performing **linear regression** using *scikit learn* is **quite simple**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113184053463.png)

- `LinearRegression` class based on `scipy.linalg.lstsq()` function (**least squares**)
  - Can **directly call**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113184205147.png)

- The function computers $\hat{\theta} = X^{+}y$ 
  - Where $X^{+}$ is the *pseudoinverse* of $X$ 

https://www.youtube.com/watch?v=vXk-o3PVUdU

https://www.youtube.com/watch?v=02QCtHM1qb4

https://youtu.be/PjeOmOz9jSY

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113200708803.png)

- Use `np.linalg.pinv()` to compute the **pseudoinverse** *directly.* this shown here uses svd to calculate approx svd value.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113200809715.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117020525659.png)

- Computed using a **standard matrix factorization method**
  - Called the **==singular value decomposition==** that decomposes the **training set** $X$ into the **matrix multiplication** of three matrices $U \Sigma V^{T}$ `np.linalg.svd` 
    - The **pseudoinverse** computed as $X^{+} = V \Sigma^{+} U^{T}$ 
- To compute $\Sigma^{+}$ 
  - Algorithm takes $\Sigma$ and sets to **zero** all the **values** that are **smaller than** a **tiny threshold**
  - Replaces all **non zero values** with the *inverse* then finally **transposing the resulting matrix**
- More *efficient* than normal equation complexity.
  - Handles **edge cases** nicer.
    - Normal equation may not work if the matrix $X^{T}X$ is **not invertible** *singular* such as if $m < n$ 
      - Or there exists some **redundant features**
        - The **pseudoinverse** is **always defined**.

### Computational Complexity

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117021028000.png)

### Gradient Descent

- Idea is **minimizing** a *cost function* to find the **optimal solution** to *various problems*

---

- Goes in the direction where the **gradient** is the **descending**
  - Once the **gradient is zero** then we reach a **minimum**

---

- Start by filling $\theta$ (*parameter vector*) with **random values** (*random init*)
  - Then **gradually improve** with *small steps*
    - Until it **converges** at a **minimum**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117021440552.png)

- Important **hyperparameter** for this is the *learning rate* which *determines* the **step size**
  - If **too small** $\to$ it must **iterate many times** in order to **converge** over a *long time*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117021545329.png)

- If **too high**
  - You may **jump the valley** and end up on **wrong side**
    - May cause a **divergence** with **larger / large values** with *no good solution*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117021654770.png)

- Not every *cost functions* look like **regular bowls**
  - Can be *ridges / holes / plateaus* and various **irregular terrain**
    - This makes **convergence** to the *minimum* quite **hard**
- If the *random init* starts on the **left**
  - Converges to a *local minimum* which is **not as good** of course.

- If it starts on the **right** 
  - Then it takes **a long time** to *cross the plateau*
    - And if you **stop too early** you *never reach* the **global minimum**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117022219342.png)

- The $MSE$ *cost function* for **linear regression**

  - Has some *convex function*

    <img src="https://www.probabilitycourse.com/images/chapter6/Convex_b.png" alt="Jensen&#39;s Inequality" style="zoom:50%;" />

    - Therefore if you **pick any two points** on the *curve*
      - The *line segment* that **joins them** will **never cross the curve**
        - This implies a **single global minimum**

  - Also has a **continuous function** with a *slope* that **changes abruptly**

    - https://en.wikipedia.org/wiki/Lipschitz_continuity.

- These *two facts* have consequence:

  - GD is **guaranteed** to reach some *arbitrarily close* value to the **global minimum**
    - If you *wait long enough* with a **reasonable** *learning rate*

- The *cost function* has the **shape of a bowl**

  - May be **elongated** for *different scales*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117022639779.png)

---

- The **left GD** goes *straight to the minimum* 
  - Therefore **reaches** it *quickly
- On the **right GD** 
  - Goes in direction **almost orthogonal** to the **direction** of the **global minimum**
    - Ends with a **long march** down an **almost flat valley**
      - Eventually reaches a **minimum** after a **while**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117022924601.png)

- Also illustrates that **training a model** means searching for a **combination** of *model parameters*
  - That will *minimise* some **cost function** (*over the training set*)
    - A search in the models *parameter space* 
      - The **more parameters** $\to$ the **more dimensions** and thus **harder the search**
- The *cost function* is *convex* in **linear regression** therefore the *target* is **always downwards**
  - Thus the *dimensions* does *not matter*.

#### Batch Gradient Descent

- Must compute the **gradient** of the *cost function* **wrt** to every parameter $\theta_{j}$ 
- Must calculate the **cost function change** as each *parameter* $\theta_j$ is *adjusted slightly*
  - The **partial derivative**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117023450438.png)

https://sebastianraschka.com/faq/docs/mse-derivative.html

<img src="https://cdn.discordapp.com/attachments/309758102582984705/910358882054991922/image0.jpg" alt="img" style="zoom: 33%;" />

- Instead of **computing** the *partial derivatives* *individually*, compute in **one go**.
  - Use :

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117024252086.png)

- They just denoted $X$ as the **matrix of combined x  parameters** and vice versa for **weights / truth values**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117024442143.png)

- Once you have the *gradient vector*
  - Which points **uphill**
    - You just go in the **opposite direction** to *go downhill*
- Therefore subtracting $\nabla_{\theta}MSE(\theta)$ from $\theta$ 
  - This is where our **==learning rate==** $\eta$ comes into **play**
    - *Multiply* the **gradient vector** by $\eta$ to determine the **size** of the *downhill step*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117024731220.png)

```python
eta = 0.1 # learning rate
n_iterations = 1000
m = 100
theta = np.random.randn(2,1) # random initialization

for iteration in range(n_iterations):
	gradients = 2/m * X_b.T.dot(X_b.dot(theta) - y)
	theta = theta - eta * gradients
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117024904568.png)

- This is **equivalent** to the **normal equation** 
  - What if we used a *different learning rate* $\eta$:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117024953814.png)

- On the **left** $\to$ *too low* $\to$ reaches after a **long time**
- Middle $\to$ relatively **optimal**
- Right $\to$ *too high* and **diverges** and moving **further** away on *each step* from the solution.

---

- Use **grid search** to find the **optimal** value for the **learning rate**
- May wish to **limit the iterations** so *grid search* can **eliminate models** that are take **too long to converge**
- Often choose a **very large** value of **iterations** and only *interrupt* when the **gradient vector** becomes **tiny**
  - Reaching the number $\epsilon$ **==tolerance==** 
    - This has *almost* reached the **global minimum**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117025429633.png)

#### Stochastic Gradient Descent

- Batch uses the **entire dataset** at *once* to compute the **gradients** of the *step*
  - Therefore is **very slow** on **large datasets**
- **Stochastic gradient descent** picks a *random instance* of the **training set** at **each step**
  - Computes the **gradient** based on this *single instance*
    - Massively improving performance and thus **feasible** for use on **larger training sets**
      - As only **one instance** must **be in memory** at a time. (*out of core algorithm*)

---

- Its **random** in *nature*
  - Therefore **less regular**
    - The *cost function* will **bounce up / down** 
    - *Decreases* only **on average**

- Over time it ends **close to minimum**
  - Continues to **bounce** at bottom
    - Once it **ends** these **final parameter values** are *good* but *sub optimal*.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117030037856.png)

- With an **irregular cost function**
  - This *helps* the algorithm *escape* from **local minima**
    - Therefore has **greater chance** to locate a **global minimum** to **batch gradient descent**
- To balance the **randomness** and **escaping from local minimum**
  - We can **reduce learning rate** *gradually*
    - Start out **large** therefore **progress faster** and *remove local minimum*
      - Then *get smaller* and **more precise**
        - Similar to **simulate annealing**

- Function that *determines* the *rate* at **each iteration** is the **==learning schedule==**
  - If reduce **too fast** $\to$ stuck in **local minimum**
  - **too slow** $\to$ may *jump* in some **local minimum** and *not get out* before the **training is halted**

```python
## stochastic gradient descent using simple schedule

n_epochs = 50
t0, t1 = 5, 50 # learning schedule hyperparameters

def learning_schedule(t):
	return t0 / (t + t1)

theta = np.random.randn(2,1) # random initialization

for epoch in range(n_epochs):
	for i in range(m):
		random_index = np.random.randint(m)
		xi = X_b[random_index:random_index+1]
		yi = y[random_index:random_index+1]
		gradients = 2 * xi.T.dot(xi.dot(theta) - yi)
		eta = learning_schedule(epoch * m + i)
		theta = theta - eta * gradients
```

- Convention we **iterate** via *rounds* of $m$ iterations
  - Each round is an **==epoch==**
- The **batch gradient descent** *code* iterated **1000 times** via the **whole training set**
  - This code only goes through **50 times** and reaches a **fairly good solution**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117031006594.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117031119996.png)

- Instances are **selected randomly**
  - Some may be **selected many times** while some **not at all** 
- Ensure the **algorithm** goes over **each instance** at **each epoch**
  - You can **shuffle the training set** (*features / labels too*) 
    - Go through **instance by instance**, then *shuffle again*
      - This **generally converges slower**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117031358542.png)

- To **perform linear regression** using **SGD**
  - Use the `SDGRegressor` which defaults at **optimising** the *squared error cost function*
    -  Following code **runs** at a *max* of $1000$ *epochs* (`max_iter = 1000`) 
    - Or until the **loss drops** to less than $1e-3$ during **one epoch** this is the **tolerance** `tol=1e-3`  
    - Starting with a **learning** rate of $0.1$ `eta0=0.1`
    - Using **default learning schedule**

```python
from sklearn.linear_model import SGDRegressor
sgd_reg = SGDRegressor(max_iter=1000, tol=1e-3, penalty=None, eta0=0.1)
sgd_reg.fit(X, y.ravel()) 
```

- Finds solution **quite close** to the one **returned** by the **normal equation**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117032200872.png)

#### Mini batch gradient descent

- At **each step** $\to$ instead of **computing gradients** based on the **full training set** as in **batch GD** or a **single instance**
  - **Mini batch** focuses on **small random sets** called *mini batches*
    - obtain **hardware optimisation** especially when using **GPUs**.

---

- The *algorithms progress* in the **parameter space** is much *less erratic* as opposed to **SGD**  
  - Especially with *fairly larger mini batches* 
    - As a **result** $\to$ *mini batch* **GD** ends up walking **a bit closer** to the **minimum** than **SGD**
      - May be **harder to escape** from *local minima*
- Figure below shows the **paths** taken by the **three gradient descent algorithms** in *parameter space* during **training**
  - All end **near minimum** however *mini batch*  and *stochastic* walk about with **batch just stopping**
    - The **batch** of course takes a **while** 
- Using a **good schedule** can help **mini batch GD** reach the **minimum properly**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117161024038.png)

- Compare the **algorithms** discussed so far for **linear regression**
  - Recalling that $m$ is the **number** of **training instances** and $n$ is the **number of features**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117161345898.png)

## Polynomial Regression

- If data is **more complex** than some **simple straight line** 
  - Use a **linear model** to *fit* some **nonlinear data**
- Simple method of **doing this** is adding **powers** of *each features* as *new features*
  - Then train some **linear model** on this **extended** *set of features*
    - This is **==polynomial regression==** 

---

- Looking at an **example first**
  - Generate some **nonlinear data**  based on some **non linear quadratic equation**.
    - using some *noise also*

```python
m = 100 # number of training instances 
X = 6 * np.random.rand(m, 1) - 3 # 6 * random array of shape m x 1 , then - 3
y = 0.5 * X**2 + X + 2 + np.random.randn(m, 1) # 0.5x^2 + x + (2 + noise)
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117162433575.png)

- A **straight line** can **never fit this**
  - Therefore use *scikit* **Polynomial Features** class to *transform* our **training data**.
    - Adding the **square** (*2nd degree polynomial*) of **each feature** in the **training set** as *new features* 
      - In this case there is just **one feature**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117162657120.png)

- `X_poly` contains the **original features** of the `X` plus the **square** of this **feature**
  - Now you can fit the `LinearRegression` model to this **extended training data**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117162846825.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117162943663.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117163007974.png)

- Gaussian meaning a **normal distribution.** 

---

- With **multiple features**
  - Can use `PolynomialRegression` to find **relationships** between these features, of which **plain linear regression cannot do**.
    - This is **possible** via the fact that `Polynomial Features` adds **all combinations** of *features* up to some **given degree**

- If there are **two features** $a$ and $b$ 
  - Then `PolynomialFeatures` of `degree = 3` would **not only add** up to the features $a^2, a^3, b^2$ and $b^3$ 
    - but also combinations $ab, a^2b, ab^2$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117163524572.png)

- Note the bias counts as a feature in this equation $x^0$ that would be

### Learning Curves

- If you perform **high degree polynomial regression**
  - Likely *fit* the **training data** better than with **linear regression**.
- Figure below **shows** some *300 degree polynomial* which is clearly **overfitting**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117165702479.png)

- The *quadratic model* is the **best generalized model** for **this regression problem**
  - As its clearly a **quadratic generated problem**.
- Dont know the **exact** the **exact degree** of teh **model** we are attempting to **fit** therefore we must select the **correct complexity** as to **not overfit / underfit**
- Mentioned earlier we can use **cross validation** for this case 
  - If a **model performs** wells on the **training data** but *generalizes* poorly according to **cross validation metrics** 
  - If it performs **poorly on both** then its **underfitting**

---

- Another option is the **==learning curves==**
  - This is the **plot** of the **models performance** on the *training set* and the *validation set* as a **function** of the **training** *set size* or the **training iteration**
- To **generate the plots**
  - Just **train the model** *several times* on *different sized subsets* of the **training set**
- Following code defines a **function** that plots the **learning curve** of *a model* given some **training data**.

```python
from sklearn.metrics import mean_squared_error
from sklearn.model_selection import train_test_split

# takes in some model linear regression / polynomial and the data features and labels
def plot_learning_curves(model, X, y):
	X_train, X_val, y_train, y_val = train_test_split(X, y, test_size=0.2)
	train_errors, val_errors = [], []
		for m in range(1, len(X_train)):
			model.fit(X_train[:m], y_train[:m])
			y_train_predict = model.predict(X_train[:m])
            y_val_predict = model.predict(X_val)
			train_errors.append(mean_squared_error(y_train[:m], y_train_predict))
			val_errors.append(mean_squared_error(y_val, y_val_predict))
            
plt.plot(np.sqrt(train_errors), "r-+", linewidth=2, label="train")
plt.plot(np.sqrt(val_errors), "b-", linewidth=3, label="val")
```

- Looking at lines of **plain linear regression**

```python
lin_reg = LinearRegression()
plot_learning_curves(lin_reg, X, y)
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117170816524.png)

- Performance on the **training data**
  - When there are just **one or two instances** in the **training set**
    - The model **perfectly fits**, therefore starting the **curve** at $0$ 
- As **new instances** are *added*
  - It become **impossible** for the *model* to *perfectly fit*
    - As its **noisy** and **nonlinear**
      - Error increases till it **reaches** some *plateau* where **new instances** have *little to no effect*.
- Looking at the **validation performance**
  - When trained on a **few instances** its clearly **incapable** of *generalisation* 
    - Therefore the **validation error** is **quite big**
      - As the *model* is **shown** more **training examples** and learns therefore the **validation error** *slowly reduces*
        - leading to *same plateau* issue with **training.**

- These **learning curves** are typical of **underfitting** reaching this *plateau* and they are **fairly close / high**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117172635330.png)

- Look at the **learning curves** of a $10^{th}$ degree **polynomial** on the *same data*

```python
from sklearn.pipeline import Pipeline

polynomial_regression = Pipeline([
		("poly_features", PolynomialFeatures(degree=10, include_bias=False)),
		("lin_reg", LinearRegression()),
	])

plot_learning_curves(polynomial_regression, X, y)
```

- These **learning curves** look *similar to the previous ones*
  - There are **2 main differences**
    1. The **error** on the *training data* is much **lower** than with **linear regression** model
    2. There is a **gap between the curves** , therefore the model **performs significantly better** on **training data** than **validation** which is **overfitting**
       - using a *larger training set* $\to$ the **two curves** *continue to get closer*.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117172806533.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117173011122.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117173134786.png)

## Regularized Linear Models

- Good method of **reducing overfitting** is to *regularize* the *model* 
  - This means to the **constrain it**
    - The *fewer degrees of freedom* $\to$ the *harder* it is **to overfit**
- Example $\to$ a simple way to **regularize** the **polynomial model**
  - Is *reducing* the *number* of **polynomial degrees**
- For some *linear model*
  - **==regularization==** is achieved by **constraining** the *weights* of the **model**
- Three models for **constraining the weights**.

### Ridge Regression

https://www.youtube.com/watch?v=Q81RR3yKn30

- This is **regularized version** of **linear regression** 
  - using some *==regularization term==* 

$$
\Large \alpha\Sigma_{i=1}^n \theta^2_i
$$

- This is **added** to the **cost function**
  - This *forces* the **learning algorithm** to *fit the model* but also **keep weights** as **small as possible**.
    - As it **sums** the **squares** of *each weight* according to some *parameter* $\alpha$ to control the **degree** it has in **cost function relevance**.
-  Only add this **during training**
  - Once the *model is trained*
    - Must **evaluate** the *models performance* using the **unregularized performance measure.**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117174257595.png)

- The **hyperparameter** $\alpha$ controls how much **you wish to regularize the model**
  - If $\alpha = 0$  then its **linear regression**
  - if $\alpha$ is **large** then all **weights** end up **close to zero** 
    - Resulting in **flat line** via the **datas mean**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117174554831.png)

- The bias term $\theta_0$ is **not regularized** 
  - Define $w$ as the **vector** of **feature weights** $\theta_1 \to \theta_n$ 
    - The *regularization term* is equal to $\large\frac{1}{2}(|| w||_2)^2$ 
    - Where $|| w||_2$ is the $l_2$ **norm ** of the **weight vector**
- For **gradient descent**
  - just add $\alpha w$ to the **MSE gradient vector**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117182353649.png)

- Figure below **shows** some *ridge models* trained on **some linear data** using a *different* $\alpha$ value

  - On the **left** $\to$ plain **ridge models** used for *linear predictions*

  - On the **right** $\to$ data first **expanded** using `PolynomialFeatures(degree = 10)` 

  - Then scaled using `StandardScaler` 

- Ridge models are applied to **resulting features**

- This is **polynomial regression** with **ridge regularization** 

- Increasing $\alpha$  leads to **flatter** (*less extreme , more reasonable*) predictions

  - Reduces the **models variance** but *increases* its *bias*.

---

- Like **linear regression**
  - Can perform **ridge regression** via computation a *closed form* equation or **gradient descent**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117183230788.png)

- Equation for **ridge regression** with a *closed form solution*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117183453216.png)

- Perform **closed form solution** using *matrix factorization method* by *Cholesky*

https://www.youtube.com/watch?v=TprfUB3nI8Y

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117184602030.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117184613015.png)

- Using **stochastic gradient descent**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117201521490.png)

- The **penalty** *hyperparameter* sets the **type of regularization term** to use
  - *Specifying* the *"l2"* indicates you want **SGD** to add a **regularization term** to the **cost function** equivalent to **half the square** of the $l_2$ **norm** of the **weight vector**
    - This is just **simple ridge regression**.

### Lasso Regression

https://www.youtube.com/watch?v=NGf0voTMlcs

- *Least absolute shrinkage* and *Selection Operation regression* or **Lasso regression**.
  - Is another **regularized version** of *linear regression*
    - just as with **ridge regression** it adds some *regularization term* to the **cost term** to the **function**
      - Uses the $l_1$ norm of the **weight vector** instead of **half** the **square** of the $l_2$ **norm**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117202255128.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117202741204.png)

- An **important characteristic** of **lasso regression** is that it often **completely eliminates** the *weights* of the **least important features** as in *set to zero*
  - The **dashed line** in the *right* $\to$ looks **quadratic** and *almost linear*
    - Every **weight** for the **high degree** *polynomial features* are **equal to zero**
      - *Lasso regression* thus **automatically performs** *feature selection* and **outputs** a **sparse model** (a *few nonzero weights*)
- Can gain **get** a **sense** of why *this* is the **case** by looking at figure below

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117203721237.png)

- On **top left plot** the *background contours* (**ellipses**) *represent* an **unregularized** **MSE** *cost function* ($\alpha = 0$) 

  - The **white circles** show the **batch gradient descent** with **this cost function**

  - The **foreground contours** (**diamonds**) represents the $l_1$ *penalty* and the **triangle** show the **BGD** *path* for the **penalty only** ($\alpha \to \infty$) 

  - The **first path** reaches $\theta_1 = 0$ 
    - Then **rolls** down a **gutter** until it **reaches** $\theta_2 = 0$ 
- For the **top right plot**
  - The **contours** are the **same cost function** for $l_1$ penalty with $\alpha=0.5$ 
  - The **global minimum** is on $\theta_2 = 0$ axis
  - **BGD** first reaches $\theta_2 = 0$ then **rolls** down the **gutter** until it *reaches* the **global minimum**
    - The **two bottom plots** show the **same** *thing* but uses an $l_2$ penalty instead
  
  - The **regularized** *minimum* is *closer* to $\theta = 0$ than **the unregularized minimum**
  
  - but the **weights** do *not get fully eliminated.*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117204531964.png)

- Lasso *cost* function is **not differentiable** at $\theta_i = 0$ for $i = 1,2, \cdots , n$ 
  - But **gradient descent** works fine if you use a *subgradient vector* $g^{15}$ instead when any  $\theta_i = 0$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117205008258.png)

- Code using the **lasso class**
- Alternative use is `SGDRegressor(penalty = "l1")` 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117205053279.png)

https://www.youtube.com/watch?v=Xm2C_gTAl8c - ridge vs lasso

- Summary lasso regression can eliminate weights and reach **0 global minimum** directly with a **straight line**.
- So it can remove **redundant variables** that have **n**
- So use **lasso regression** when you have a **few variables** that are similar and lots of **redundant ones**
- but **ridge regression** performs better if they are **large variables** consistently as **l2 norm** favours these over **l1 norm** to reach overall **better fit**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117214142735.png)

- so weight has no effect at this lambda value for lasso regression as the global minimum has a sharp cut off point as seen on the graph where its exactly at 0.

### Elastic Net

- **Elastic net** is a *middle ground* between **ridge / lasso** regression
  -  Is a **simple mix** of both **ridge / lasso** *regularization* terms
    - Control the **mix ratio** $r$ 
- When $r = 0$ **elastic net** is *equivalent* to **ridge**
- $r=1$ its **equivalent** to **lasso**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117214359693.png)

- When should you use **plain linear regression** / Choice of **ridge / lasso / elastic net**
  - Preferred to have **some regularization often** 
    - *Ridge* being the **default** 
    - If you see a **few features** that are **useful** and many **useless values** then **prefer** the *lass / elastic net* as they **reduce** the *useless features* to *zero*
- Prefer **elastic net** over **lasso** to **control** how *erratic* the *lasso function is* when the **number of features** is *greater* than the *number* of **training instances** 
  - Or when **several features** are *correlated strongly* 
  - $r=$ `l1_ratio` 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117214711382.png)

### Early Stopping

- Stop **training** as soon as the **validation error** reaches a **minimum**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117214815952.png)

- Shows a **complex model** (*high degree polynomial regression*)
  - Trained using **batch gradient descent**
    - As *epochs* pass $\to$ the **algorithm learns** and its **prediction error** $RMSE$ on the **training set** *naturally decreases*
      - Thus the **prediction error** on the **validation set**.
- However 
  - Afer some **time** the **validation error** *stops* *decreasing* and **actually starts** to **go back up** 
    - This **indicates** that the **model has started** to *overfit* the **training data** 
      - With **early stopping** you just **stop training** as soon as the **validation error** reaches the *minimum*
- It is such a **simple / efficient regularizaiton technqiue**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117215546747.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117215603044.png)

- Basic implementation of **early stopping**

```python
from sklearn.base import clone
# prepare the data
poly_scaler = Pipeline([
		("poly_features", PolynomialFeatures(degree=90, include_bias=False)),
		("std_scaler", StandardScaler())
	])

X_train_poly_scaled = poly_scaler.fit_transform(X_train)
X_val_poly_scaled = poly_scaler.transform(X_val)

# warm_start When set to True, reuse the solution of the previous call to fit as initialization, otherwise, just erase the previous solution. See the Glossary.
sgd_reg = SGDRegressor(max_iter=1, tol=-np.infty, warm_start=True,
						penalty=None, learning_rate="constant", eta0=0.0005)

minimum_val_error = float("inf")
best_epoch = None
best_model = None

# run epochs
for epoch in range(1000):
    
    # fit to the model
    sgd_reg.fit(X_train_poly_scaled, y_train) # continues where it left off
    
    # predict
    y_val_predict = sgd_reg.predict(X_val_poly_scaled)
    
    # calculate loss
    val_error = mean_squared_error(y_val, y_val_predict)
    
    #if loss < than certain thresold update this as the best model and save it
    if val_error < minimum_val_error:
        minimum_val_error = val_error
        best_epoch = epoch
        best_model = clone(sgd_reg)
```

- Note with `warm_start = true` when the `fit()` method is **called**
  - It **just continues** *training* where it **left off** instead of **restarting from scratch**

## Logistic Regression

- **Logistic regression** is used to estimate the **probability** that some instance belongs to a **particular class**
  - If its greater than say **50%** its **accepted** for example with **ham / spam** its **passed**, else not.
    - Therefore **binary classifier**.

### Estimating Probabilities

- A **logistic regression** computes a **weighted sum** of the **input features** (*plus bias term*)
  - But **instead** of *outputting* the **result directly** like the **linear regression** model does
    - it outputs the **logistic of this result** 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117221050272.png)

- The **logistic** noted $\sigma(\cdot)$ is a **sigmoid function**
  - Outputting a **number** between **0** and **1**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117221138318.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117221146383.png)

- Once the **logistic regression** model has *estimated* the probability $\hat{p} = h_\theta(x)$ that an **instance** $x$ belongs to the **positive class** 
  - Can make its **prediction** $\hat{y}$ easily

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117221340374.png)

- Notice that $\sigma(t) < 0.5$ when $t < 0$ and $\sigma(t) \geq 0.5$ when $t \geq 0$
  - So a **logistic regression** model $1$ if $X^T\theta$ is **positive** and $0$ if its **negative**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117221622672.png)

### Training and Cost function

- Good $\to$ *now* you how a **logistic regression** model *estimates* probabilities and **make predictions**
  - How is it **trained**
    - The *objective* is to **set the parameter** vector $\theta$ so that **model estimates** the *high probabilities* for **positive instances** ($y=1$) and **low probabilities** $(y = 0)$
      - Capture by the **cost function** for a **single training instance** $x$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117223535924.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117222426585.png)

- This **cost function** makes *sense because* $-log(t)$ grows **large** as $t$ approcahes $0$ 
  - So the **cost** is **large** when the **model** estimates a **probability** close to $0$ for **positive instances**
  - Also **large** if the model estimates a **probability** close to $1$ for **negative instances**
- Alternatively $-log(t)$ is **close** to $0$ when $t$ is close to $1$ therefore the **cost** will be **close** to $0$ if the **estimate probability** is close to $0$ for a **negative instance**
- or close to $1$ for a **positive instance** 
- This is **exactly what we want**.

---

- The *cost function* over the **whole training set** is the **average cost** over **all training instances**
  - Written in a **single expression** called the **==log loss==**. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117224259112.png)

- The **bad news** is there is **no closed form equation** to compute the *value* of $\theta$ that **minimises** this *cost function*
  - There is **no equivalent** of the **normal equation**
- The **function** is a **convex ** therefore **gradient descent** is *certain to find* the **global minimum**. 
- The **partial derivative** of the **cost function** *wrt* to the $j^{th}$ model **parameter** $\theta_j$ is given in **equation**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117230504295.png)

![img](https://cdn.discordapp.com/attachments/309758102582984705/910680921554968576/image0.jpg)

- or look at this link nothing at a bit different notation but same concept. https://math.stackexchange.com/questions/477207/derivative-of-cost-function-for-logistic-regression
- $\hat{p}^{(i)}$ is the **shorthand** for the **logistic function** that they used in the **derivative**.
- final linal just do the algebra and remember to make it (p-y)x whcih means taking a negative out which negates the outer sign, 

- Looks similar to **MSE partial derivative** *cost function* 
  - For **each instance** $\to$ computes the **prediction error** and *multiplies* by the $j^{th}$ feature value
    - Then **computes** the *average* over *all training instances*
- Once you have a **gradent vector** of all the **partiald derivatives**
  - Use in the **batch gradient descent algorithm**
    - **For stochastic** $\to$ just train **one instance at a time** and **mini** just use **each batch** at a time.

### Decision Boundaries.

- Using the **iris** dataset to **illustrate logisitic regression**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118010317560.png)

- Try to **build a classifier** to detect the **iris virginica type**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118010812960.png)

- Train a **logistic regression model**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118010831001.png)

- Look at the model **estimated probabilities** for flowers with **petal widths** varying from $0$ to $3cm$  

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118011504400.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118011516390.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118011524104.png)

- Triangles range from 1.4 to 2.5
- other flowers are squares have a smaller petal width 0.1 to 0.8
- There exists some overlap
- Above 2cm the classifier is **highly confident** in it being iris.
- below 1cm its **highly confident** its *not*
- In between its **unsure**
- Selecting a class using *predict* returns the **most likely class** thus this is a **==decision boundary==** at 1.6 where they are both $50\%$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118015122982.png)

- Gives **one hot encoded** output of the **value it is**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118015240144.png)

- The **same dataset** but displaying **two features**
- Once trained **logisistic regression** can *estimate* the **probability** that a *new flower* is an **iris** based on **two featurers**
- *Dashed line* is the **decision boundary** which is **linear boundary** 
- Each **parallel line** represents the **points** where the *model outputs* some **specific probability** 
  - 15% from bottom  left to 90% top right.
    - All flowers beyond top right have **90% chance** of being right.

- Just as with **other linear models**
  - Logistic regresion can be **regularised** using $l_1$ or $l_2$ penalties
    - $l_2$ is the default.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118021820385.png)

## Softmax Regression

- The **logistic regression** model can be **generalized** to support *multiple classes directly*
  - Without **training** and **combining** multiple **binary classifiers** as mentioned in **chapter 3**
    - This is **==softmax regression==** or **multinomial logistic regression**

---

- Given instance $x$ 
  - Softmax regression computes a score $s_k(x)$ for each class $k$ then **estimates** the **probability** of each class by applying the **sofmax function** (*normalized exponential*)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118022131752.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118022139609.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118022445957.png)

- Pass each **computed score** of *every class* for instance $x$ and estimate the **probability** $\hat{p}_k$ that the **instance** belongs to *class* $k$ by running each score via **softmax function** 
  - Computes exponential then takes the an average over **all other classes**.
- Scores are called **logits** or **==log odds==** 

---

- This just predicts the class with the highest probability 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118022508831.png)

- $argmax$ returns value of variable to **make** this function as **large as possible**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118022559461.png)

- Calculate loss with **cross entropy**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118022647454.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118022654608.png)

- Notice that when there are **two classes** $K=2$ 
  - This **cost function** is equivalent to the **logistic regression** cost function

---

- The **gradient vector** of this **cost function** with regards to $\theta^{(k)}$ given by this **equation**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118025634623.png)

- Now computer the **gradient vector** for every class using **gradient descent**
  - Or any other **optimization algortihm**
    - To find the matrix $\Theta$  that **minimizes** the *cost function*.

---

- Use **softmax regression** to *classify* the flowers to **three classes**
- `LogisticRegression` uses **OvA** as default on **more than two classes**
  - Set class hyperparameter to `multinomial` for **softmax regression**
- Specify a **solver** also as $lbfgs$ 
- Applies a $l_2$ *regularization* by default, controlled with $C$ variable.

```python

X = iris["data"][:, (2, 3)] # petal length, petal width
y = iris["target"]

softmax_reg = LogisticRegression(multi_class="multinomial",solver="lbfgs", C=10)
softmax_reg.fit(X, y)
```

- Therefore **next time** you find an iris with 5cm long and 2cm wide petals
  - Can ask the **model** to **tell us the type** with high probability

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118034336746.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118034345172.png)

- Show resulting **decision boundaries**
  - Represented with **colours**
    - Decision boundaries between two classes is **linear**
- Also shows probabilities for the **iris versicolor class** as *cruved lines*
- The model can **predict** a *class* that has an **estimated** probability below $50\%$ 
  - Example $\to$ at the **point** where all **decision boundaries** meet
    - All classes have **equal estimated probability** of **33%** 

