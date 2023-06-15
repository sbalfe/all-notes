# Support vector machines

- **SVM** $\to$ portable of **performing** some *linear / nonlinear* **classification** / **regression** and even **outlier detection**.
  - One of the **most popular models** in *machine learning*.

## Linear SVM Classification

- Figure below shows the **iris dataset** 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211121221935320.png)

- The **two classes** can be *easily separated* with some **straight line**. 
  - They are *linearly separable*
- Left shows **decision boundaries** of *three potential classifiers* 
  - The **dashed line** is *so bad* it does not **even properly seperate** 
- Other two are **perfect** on this **training set** but *not well on new instances*
- The **solid line** in the **plot** on the *right represents* the **decision boundary** of an **SVM classifier**
  - This **line** not only **separates** the **two classes** but also **stays** as **far away** from the **closest training instances** as **possible**
    - Think of an **SVM classifier** as *fitting* the **widest possible street**
      - This is represented by the *dashed lines* that is **between the classes**
        - Called the ***==large margin classification==***

---

- Adding more **training instances** *off the street* will **not affect** the *decision boundary* at all
  - Its **fully determined** / **supported** by the *instances* located on the **edge of the street** 
    - These instances are called the **==support vectors==** and are *circled* in the **image above**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211121224449158.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211121224602577.png)

## Soft Margin Classification

- If we **impose** that **all instances** be *off the street* and on the *right side*.
  - This is **hard margin classification** 
    - Two main **issues**:
      1. Only works with **linearly separable data**.
      2. Quite *sensitive* to **outliers**.
- Figure below shows *iris dataset* with **just one additional outlier**
  - On the left $\to$  its **impossible** to locate some **hard margin**, because the **outlier** would **split** the **data**.
    - And for the **right** $\to$ on the **decision boundary** ends up in **very different** from the **one we saw before** with **no outlier** and will **not generalise** well *likely*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211121225657267.png)

- To **avoid** these **issues**
  - Preferable to use a **more flexible model**.
    - The *objective* is to find a **good balance** of keeping the **street** as *large as possible* whilst **limiting** the **margin violations** (instances that end up in the middle of the street or even on the wrong side)
      - This is **==soft margin classification==**. 
- In *scikit-learn* SVM classes
  - Can **control** this balance using the `C` **hyperparameter** 
    - A *smaller* $C$ value leads to a **wider street** but *more margin violations*
- Figure below Shows these **decision boundaries** and the **margins** of *two soft margin* $SVM$ classifiers on a *nonlinearly separable dataset*.  
  - On the **left** $\to$ using a **low C value** $\to$ create a **large margin** but makes many instances **fall on the street** 
  - On the **right** $\to$ using a **high C value** $\to$ classifier makes **fewer margin violations** but ends with **smaller margin**
- Likely the **first classifier** is able to **generalize better** 
  - on this **training set** $\to$ it makes **fewer prediction errors** since most **of the margin violations** are on the **correct side** of the **decision boundary**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211122171557393.png)

- Remember the middle line is the **classifier** its defined as the **midpoint** between the **two hyperplanes** decided on by the **support vectors**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211122173527007.png)

- Code below 

```python
import numpy as np
from sklearn import datasets
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.svm import LinearSVC

# load iris dataset
iris = datasets.load_iris()
X = iris["data"][:, (2, 3)] # petal length, petal width
y = (iris["target"] == 2).astype(np.float64) # Iris-Virginica

# add SVM to the pipeline with feature scaling to increase the street size to be 
# more accurate to the dataset varying feature sizes to obtain a better generaliser.
svm_clf = Pipeline([
		("scaler", StandardScaler()),
		("linear_svc", LinearSVC(C=1, loss="hinge")),
	])

# fit our values to this model.
svm_clf.fit(X, y)

# predict the outcome
svm_clf.predict([[5.5, 1.7]]) 

>>>
array([1.])
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211122173830873.png)

- Could use the `SVC` class via `SVC(kernel = "linear", C = 1)` 
  - This is **much slower** , particularly with **larger datasets**, therefore not reccomended
    - Option is using the `SDGClassifier` 
      - With `SDGClassifer(loss = "hinge", alpha = 1/(m*C))`
- This applies **regular** **stochastic gradient descent** to train *linear SVM classifier* 
  - It **does not converge** as *fast* as the `LinearSVC` class
    - Can be **more useful** to *handle huge datasets* that **do not fit in memory** (*out of core training*) or to handle **online classification tasks**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211122174540063.png)

- Hinge loss - https://programmathically.com/understanding-hinge-loss-and-the-svm-cost-function/https://programmathically.com/understanding-hinge-loss-and-the-svm-cost-function/

## Nonlinear SVM Classification 

- **Linear** *SVM* classifiers are **efficient** and work in **most cases** 
  - many datasets are **not close** to being **linearly separable** however.
    - An approach to **nonlinear datasets** is *adding more features* for examples **polynomial features** (*like* in **chapter 4**) 
- For **some cases** $\to$ this may result in **linearly separable datasets**
  - Consider left plot below $\to$ **represents** a **simple dataset** with a single feature $x_1$ 
    - This is *dataset* is **not linearly separable** 
      - But adding a **second feature** $x_2 = (x_1)^2$ 
        - The **resulting dataset** is *perfectly linearly separable*.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211122182352951.png)

- Implement this $\to$ create a `Pipeline ` that contains a `PolynomialFeatures` **transformer**
  - Followed by a `StandardScaler` and a `LinearSVC` 
    - Test this on *moons dataset* $\to$ *test dataset* for **binary classification** $\to$ in which the **data points** are *shaped* as **two interleaving half circles** 
      - You can generate this dataset using the `make_moons()` function. 

```python
from sklearn.datasets import make_moons
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import PolynomialFeatures

polynomial_svm_clf = Pipeline([
		("poly_features", PolynomialFeatures(degree=3)),
		("scaler", StandardScaler()),
		("svm_clf", LinearSVC(C=10, loss="hinge"))
	])

polynomial_svm_clf.fit(X, y)
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211122182642926.png)

### Polynomial Kernel

- Adding **polynomial features** is *simple* to **implement** and works with **many branches** of *machine learning* and not just *SVMs*
  - but at a **low polynomial degree** it *cannot deal* with **very large complex datasets**. 
    - And with a **high polynomial degree** $\to$ creates a **huge number of features** thus making a **slow model**

---

- For SVMs $\to$ can apply the **kernel trick**
  - Enables us to obtain the **same result** as **adding polynomial features** 
    - Even with **high degree polynomials** *without* actually **adding them**
- Therefore there is **no combinatorial explosions** of the **number of features** since you do *not add any features*
  - This is implemented by the `SVC` class
    - Test on the **moons dataset**

```python
from sklearn.svm import SVC

# trains an svm classsifier via 3rd degree polynomial kernel
poly_kernel_svm_clf = Pipeline([
		("scaler", StandardScaler()),
		("svm_clf", SVC(kernel="poly", degree=3, coef0=1, C=5))
	])
poly_kernel_svm_clf.fit(X, y)
```

- This is **shown** on the **left** below
- On the **right** is another **SVM classifier** with a **10 degree polynomial** which is *clearly overfitting*
- If **underfitting** , increases
- The **hyperparameter** `coef0` controls how **much the model** is *influenced* by **high degree polynomials** versus **low degree polynomials**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211122183935754.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211122185645634.png)



- note $r$ is the **coefficient** used in the **polynomial kernel**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211122193709406.png)

### Adding Similarity features

- Alternative method to **tackle** the **nonlinear problems** is *adding features* computed via a *similarity function*
  - This **measures** how *much* each **instance** resembles a **particular landmark**
- Example $\to$ take some **one dimensional** dataset discussed earlier
  - Add **two landmarks** at $x_1 = -2$ and $x_1 = 1$ 
    - Then define the **similarity function** to be the **gaussian** *radial basis function* with $\gamma = 0.3$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211122192538711.png)

https://en.wikipedia.org/wiki/Radial_basis_function_kernel

https://www.youtube.com/watch?v=Qc5IyLW_hns& - gaussian RBF

https://www.youtube.com/watch?v=Toet3EiSFcM - polynomial kernel.

- Basically takes two points $x, l$ and calculates the **exponential** of the **euclidean distance** multiplied by a **gamma scale factor** to determiner the **influence**. exponential i presume is because its always positive and increasing/decreasing so its a direct scale upwards / downwards without reaching 0.

- Forms a **bell shaped function** that varies from $0$ (features *far from the landmark*)$\to$ $1$  (*at the landmark*)
  - Now we can **compute** the **new features**
- Example $\to$ look at instance $x_1 = - 1$ 
  - Located at **distance** $1$ from the **first landmark** and **distance** $2$ from the **second landmark**
    - Therefore its **new features** are $x_2 = e^{-0.3 \times1^2} \approx0.74$ and $x_3 = e^{-0.3 \times 2^2} \approx 0.30$ 
- The **right plot** shows the **transformed dataset** (*dropping the original features*)
  - Its now **linearly separable**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211122193949470.png)

> Hard to visualise the **entire dimension space** but **intuitively** it should make sense how its **increasing dimensionality** to enable **linear separation**.

---

- May wonder how to **select landmarks**
  - Basic approach is **creating** a **landmark** at the **location** of *each instance*
    - Thus creating **many dimensions** and increasing the **chances** that the **transformed set** is *linearly separable*
- The **downside** is now that a **training set** with $m$ instances and $n$ features (*assuming your drop the original features*)
  - transformed to a training set  with $m$ features / instances
    - If the **set is large** $\to$ end up with **equally large number of features**

### Gaussian RBF Kernel

- The **similarity features** method can be *useful* with **any machine learning algorithm**
  - May be **computationally expensive** to *compute* all the **additional features**, particularly for *larger training sets*.
- However the *kernel trick* helps this for **SVM's**
  - Makes it *possible* to obtain a **similar results** as if you had **many similarity features**
    - Without adding **them** in sense
- Attempt on the **Gaussian RBF kernel** using the **SVC class**.

```python
rbf_kernel_svm_clf = Pipeline([
        ("scaler", StandardScaler()),
        ("svm_clf", SVC(kernel="rbf", gamma=5, C=0.001))
	])

rbf_kernel_svm_clf.fit(X, y)
```

- Model shown on figure below on bottom left.
- Other plots show models with **different hyperparameters** of $\gamma$ and $C$ 
  - *Increase* of $\gamma$ $\to$ makes the **bell shape** become **narrower**
    - Thus each **instances** *range of influence* becomes **smaller**
      - The **decision boundary** ends up *more irregular* and **wiggles around individual instances**.
  - *decrease* of $\gamma$ $\to$ makes **bell shape** become **wider**
    - Therefore increasing the **influence** and thus a **smooth decision boundary**.
  - Thus $\gamma$ acts as a **regularization hyperparameter**
    - If the model **overfits** $\to$ *reduce* else **increase** 
      - Much like the $C$ value.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124143806121.png)

- Other **kernels** exist but *are rare*
  - Some may be **specialized** for **set data structures**
    - *String kernels* $\to$ sometimes used when **classifying text documents** or **DNA sequences**.
      - e.g. $\to$ using the *string subsequence kernel* or kernel based on **levenshtein distance**

https://en.wikipedia.org/wiki/String_kernel

https://en.wikipedia.org/wiki/Levenshtein_distance

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124144559540.png)

### Computational Complexity

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124144728542.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124144705241.png)



![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124144649828.png)

## SVM Regression

- The **SVM** is *quite versatile* 
  - Supports **linear / nonlinear classification**.
- Also supports **linear / nonlinear regression**.
  - The **idea** is to *reverse the objective*
    - Rather than **fitting** the **largest possible street** of *two classes* while **limiting margin violations**.
      - **SVM regression** attempts to fit **as many instances** *on* the *street* while **limiting margin violations** (*off the street*) 
      - The **width** of the *street* controlled by **hyperparameter** $\epsilon$ 
- Figure below shows **two non linear SVM regression models** 
  - Trained on *some random linear data* 
  - One has a **larger margin** $\epsilon = 1.5$ and other a **small margin** $\epsilon = 0.5$. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124145606473.png)

- Adding **more training instances** *within the margin* does **not affect** the **models prediction**
  - Thus the model is $\epsilon$ *insensitive*. 

---

- Use *scikit learn* `LinearSVR` class to **perform linear SVM regression**.
  - The *following code* produces the **model represented** on the *left*.
    - The **training data** should be **scaled / centered first**.

```python
from sklearn.svm import LinearSVR
svm_reg = LinearSVR(epsilon=1.5)
svm_reg.fit(X, y)
```

- To Fight **nonlinear regression**
  - Just use **kernelized** **SVM** model
    - Example below shows **SVM regression** on a *random quadratic training set* 
      - Using a $2^{nd}$ *degree polynomial*.
- There is **little regularization** on the *left* (*recall* regularization means **added error** to adjust the **fitting** to prevent **over / under** etc.) 
  - Meaning a *large* $C$ value
- More on **right plot**
  - Thus a *small* $C$ value.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124150106823.png)

- Following **code** *produces* the model represented above using **scikit-learn** *SVR class*
  - This supports the **kernel trick** 
- The *SVR* class is the **regression equivalent** of the *SVC* class and `LinearSVR` class is the **regression equivalent** of the `LinearSVC` class
  - While the `SVR` class gets **much too slow** when the **training set grows large** just as with the `SVC` class.

```python
from sklearn.svm import SVR
svm_poly_reg = SVR(kernel="poly", degree=2, C=100, epsilon=0.1)
svm_poly_reg.fit(X, y)
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124155804491.png)

https://scikit-learn.org/stable/modules/outlier_detection.html

## Under the Hood

- Explains how SVMs make predictions and **how their training algorithms** work.
- https://towardsdatascience.com/support-vector-machines-dual-formulation-quadratic-programming-sequential-minimal-optimization-57f4387ce4dd#401c

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124160835802.png)

### Decision function and predictions

- The *linear SVM classifier* model **predicts** the **class** of **new instance** $x$ by *computing* said function $w^{T}x + b = w_1x_1 + \cdots w_nx_n + b$ 
  - If **positive** then the predicted class $\hat{y}$ is the **positive class** $(1)$ , else **negative class** $(2)$

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124161132864.png)

- Figure below shows the **decision function** that *corresponds* to the **model on the left** of figure 5.4

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124161222447.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124161240716.png)

- The **dashed lines** *represent* the **points** where the **decision function** is *equal* to $1$ or $-1$ 
  - They are **parallel** and at **equal distance** to the **decision boundary**.
    - Forming a **margin** *around it*.

- Training a **linear SVM classifier** *means* finding the value of $w$ and $b$ that make this **margin** as **wide as possible**
  - Whilst avoiding **margin violations** (*hard margins*) or **limiting them** (*soft margins*)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124163234750.png)

### Training objective 

- Consider the **slope** of the **decision function**
  - This is the *norm* of the **weight vector** $|w|$ 
- Dividing the **slope** by $2$ 
  - The points where the **decision function** is *equal* to $\pm1$ 
    - Are going to be **twice as far away** from the **decision boundary**
      - In other words $\to$ *dividing slope by 2* $\to$ multiplies the **margin** by $2$. 

- Explained by diagram
  - The *smaller the weight vector* $w$ the **larger the margin**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124163244665.png)

- Thus we **wish** to **minimize** $||w||$ to obtain a **larger margin** 
- However to avoid **margin violations** (*for hard margin*) 
  - Require a **decision function** $\to$ to be $>1$ for **all positive training instances** 
  - Require $<1$ for **negative training instances** 

---

- Define $t^{(i)} = -1$ for **negative instances** ($y^{(i)}=0$) 
-  $t^{(i)} =1$ for **positive training instances** ($y^{(i)}=1$)
- Then we may express **this constraint** as $t^{(i)}(w^Tx^{(i)}+b) \geq 1$ for **all instances**

---

- Therefore express the **hard margin** *linear SVM classifiers* **objective** as the **constrained optimisation problem**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124163944101.png)

- To obtain the **soft margin objective** 
  - Must introduce a **slack variable**  $\zeta^{(i)} \geq 0$  for **each instance** 
    - This measures how much the $i^{th}$ instance is **allowed** to **violate the margin**
- There are now **two conflicting objectives**
  - Minimisation of the *slack variable* for **margin violation**
  - Minimisation of $\frac{1}{2}w^Tw$ to be **as small as possible** to *increase the margin* 
- This is where we **introduce** $C$ 
  - Enables us to define this **tradeoff**  between each **objectives**
    - Giving us the **constrained optimisation problem**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124164458497.png)

> --- self notes ---
>
> so minimise the normal values but add on the C value to control the leverage of the **slack value** for the instances , which must be **minimised** to obtain the **lowest margin possible** whilst also subjecting it to not **breaking the violation** given some **slack value** to determine **by what value can it break into the margin**

>  And this must be optimised as posed the original question

### Quadratic programming

- The **hard margin / soft margin** problems are *convex quadratic optimisation* problems
  - Called $QP$ problems
- Many solvers to these issues with **complex solutions**
  - General **problem formulation shown below**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124170027572.png)

- Note the expression $Ap \leq b$ defines $n_c$ *constraints*:

$$
\large p^Ta^{(i)} \leq b^{(i)} \text{ for } i= 1,2,\cdots,n_c
$$

- Where $a^{(i)}$ is the **vector** that *contains* the *elements* of the $i^{th}$ row of $A$ 
- And $b^{(i)}$ is the $i^{th}$ element of $b$ 

---

- Easily **verify** that if you set the $QP$ parameters in the **following way**
  - Can obtain the **hard margin linear SVM classifier objective**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124170649667.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124170657068.png)

- One method to **train** a *hard margin linear SVM* is to use the **off the shelf solvers** with **preceding parameters**

  - Resulting vector $p$ contains the **bias term** $b=p_0$ and the **feature weights** $w_i = p_i$ for $i = 1,2, \cdots , n$ 
    - Can use a $QP$ solver to solve the **soft margin**

  > -- self notes -- 
  >
  > Note they flipped the sign as its solving as to select values that are lower than -1, which still reaches same results just using a flipped version of the original equality constraints, this seems to be a inequality constraints.

### Dual Problem (For kernel trick)

https://www.youtube.com/watch?v=XkpsruJC5Mk - encapsulates entire dual problem as a whole

https://youtu.be/6-ntMIaJpm0 - dual problem for svm

https://youtu.be/yuqB-d5MjZA - Lagrange multipliers basic

https://www.youtube.com/watch?v=hQ4UNu1P2kw - lagrangian in depth.

https://towardsdatascience.com/support-vector-machines-dual-formulation-quadratic-programming-sequential-minimal-optimization-57f4387ce4dd - covers the derivation of the dual form using partial derivatives and lagrangian multiplier basis, note our version down there is flipped to be a minimization so its multiplied the other way around

- Given some **constrained optimisation** problem called the *primal problem*
  - Its possible to **express** a **different** but similarity related problem called the *dual problem*
    - The **solution** to the **dual problem** often gives a **lower bound** to the *solution* of the *primal problem*.
- Under **certain conditions** $\to$ may have the **same solutions** as the *primal problem* 
  - Luckily the $SVM$ problem happens to **meet these conditions** 
    - Thus choose the *primal problem* or *dual problem* , both equivalent solutions
- Equation shows the **dual form** of the **linear** $SVM$ objective

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124181255024.png)

- Once you find the vector $\hat{\alpha}$ that **minimizes** the equation via **QP solver**
  - you can compute $\hat{w}$ and $\hat{b}$ that **minimize** the **primal problem** via:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124181432136.png)

- The dual problem is **faster** to **solve** than the **primal** when the **number of training instances** is *smaller* than the **number of features** 
  - More importantly $\to$ makes the **kernel trick possible** while the **primal does not**.

### Kernelized SVM

- Suppose you wish to apply a $2^{nd}$ degree polynomial transformation to **two dimensional training set**
  - Train a *linear SVM* classifier on the **transformed training set**
- Equation below shows the $2^{nd}$ degree polynomial mapping function $\phi$ that you **wish to apply**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124183606337.png)

- Notice that the **transformed vector** is **three dimensional** rather than **two dimensional**.
  - Look at what happens to a **couple of 2D vectors** $a$ and $b$ if we apply this $2^{nd}$ degree **polynomial mapping** and then **compute** the **dot product** of the **transformed vectors**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124183751861.png)

- The **dot product** of the **transformed vectors** is *equivalent* to the **square of the dot product** of the **original vectors**:

$$
\large \phi(a)^T\phi(b)= (a^Tb)^2
$$

- Here is the **key insight** 
  - If you **apply the transformation** $\phi$ to all **training instances** then the *dual problem* contains the **dot product** :

$$
\large \phi(x^{i})^T\phi(x^{(j)})
$$

- But if the $\phi$ is the $2^{nd}$ degree polynomial transformation
  - Then you **replace** this **dot product** simply with:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124184235121.png)

- Therefore you dont **actually transform** the **training instances** 
  - just replace the **dot product** by its **square**
    - This result stays the **same** as if you went through the **trouble of transforming** the *training set* then **fitting a linear SVM algorithm.**
- This trick makes the **whole process** more **computationally efficient**, the true *essence of the kernel trick*

---

- The function $K(a,b) = (a^Tb)^2$  is the $2^{nd}$ **degree** **polynomial** **kernel**
  - In ML $\to$ a *kernel* is just some **function** that is *capable* of computing the **dot product** $\phi(a)^T\phi(b)$ 
    - Based only on the **original vectors** $a$ and $b$ without having to **compute** or (*even know about* ) the **transformation** that takes place in $\phi$ 
- Commonly used kernels

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124184635098.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124184744839.png)

- Equation of the **dual solution** to **primal problem** above equation 5-7
  - Shows how to go from **dual solution** to the **primal solution** in the case of *linear SVM classifier*
    - But if you **apply kernel trick** $\to$ you end up with **equations** that include $\phi(x^{(i)})$ 
- In fact $\hat{w}$ must have the **same number of dimensions** as $\phi(x^{(i)})$ 
  - May be **massive / infinite** thus incomputable.
    - But you can **make predictions** without knowing $\hat{w}$ by plugging its **formulae** into the **decision function** for some new **instance vector** $x^{(n)}$ 
      - Obtaining an **equation** with *only dot products* between the **input vectors** 
        - This makes it **possible** to use the **kernel trick** once again.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124185215361.png)

- Since $\alpha^i \neq 0$ only for **support vectors**
  - Making *predictions* involves computations of the **dot product** of the **new input vector** $x^{n}$ with only the **support vectors** and *not all training instances*
    - You need to compute the **bias term** using the *same trick* $\hat{b}$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124185745021.png)

### Online SVMs

- Look at online SVM classifiers
  - Recall that **online learning** means *learning incrementally* as *new instances arrive*
- For linear **SVM classifiers** 
  - One method is using **gradient descent** e.g. using `SDGClassifier` to **minimize** the *cost function* 
    - Derived from the **primal problem**
      - This *converges much more slowly QP based methods*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124191356598.png)

- First **sum** in the **cost function** will *push the model* to have some **small weight vector** $w$ leading to a **larger margin**
- The **second sum** computes the **total** of *all margin violations*
  - An instances **margin violation** is *equal* to $0$ if its **located off the street** and *on* the **correct side**
    - Otherwise its **proportional** to the *distance* to the **correct side** of the *street* 
      - Minimizing the **term** *ensures* that the **model** makes the **margin violations** *as low as possible*

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124191712446.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124191747516.png)

