# Dimensionality Reduction

- Many ML problems involve **thousands / millions of features** for *each instance*
  - This makes **training slow**
    - Also makes it **harder to find a good solution**

---

- In **real world** $\to$ possible to **reduce the number of features** considerably
  - Turning an **intractable problem** $\to$ a **tractable one**
- Consider the **MNIST images**
  - The **pixels** on the **image** *borders* are *always white*
    - Could **drop these** from the **training set** *without loss of information*.
- Figure below **confirms** these pixels are **not important at all** for the *classification task*
- The **two neighbouring pixels** are often **highly correlated**
  - Merging to a **single pixel**
    - By taking the **mean of two pixel intensities**
      - You **will not lose much information**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211202200215954.png)

---

- Also used for **data visualisation**
  - Reducing the **number of dimensions** down to **two or three**
    - Makes it **possible** to plot some **condensed views** of a **high dimensional training** set on a **graph** 
      - Often gain **some important insights** by *visually detecting patterns*
        - Such as **clusters**.
- Data vis is required to **communicate** the conclusions to **people** who are **not data scientists**
  - In particular $\to$ makes who will  **use your results**

---

## Curse of Dimensionality

- We are used to **3D**
  - The *intuition fails* with **higher dimensions**
    - Even a **4D basis hypercube** is hard to **picture in our mind**
      - Let alone some **200 dimensional ellipsoid bent** in a **1000 dimensional space**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211202200810950.png)

- Things *behave differently* within **higher dimensional space**
  - Example $\to$ pick some **random point** in $1 \times 1$ square
    - has a $0.4\%$ chance of **being located** $<$ $0.001$ from a **border** (*very unlikely* that a random point will be **extreme** along any dimensions)
- For a **10,000 dimensional hypercube**
  - This probability is *greater than* $99.999999\%$ 
    - Most **points** in a **high dimensional hypercube** are *close to border*.

---

- More **troublesome difference**
  - Select **2 points** randomly in the **unit square**
    - The *average distance* between the points on **average** is $0.52$ 
  - Do this within a **unit cube**
    - The *average distance* is **roughly** $0.66$ 
  - Do this within some **1,000,000 dimensional hypercube**
    - The **average distance** is about $408.25$ 
      - This is **counterintuitive**
        - How can **two points** be *so far apart* when the **both lie** within the **same unit hypercube**
  - This Idea implies that **high dimensional datasets** are at *risk* of becoming **very sparse**
    - Most *training instances* are likely to be **far away from one another**.
- This means a **new instance** $\to$ is likely going to be **far away** from any **training instances**
  - Thus *making predictions* that are **much less reliable** than in **lower dimensions**
    - Since they are based on **larger extrapolations**.

> Summary > more dimensions > greater the risk  of overfitting

---

- One solution to the **curse of dimensionality**
  - Is to *increase the size* of the **training set** to reach a **sufficient density** of **training instances**
- In *practice* $\to$ the *number of **training instances*** required to **reach** a *given density grows* **exponentially** with the **number of dimensions**
  - With just **100 features** (*much less than MNIST*)
    - You would need **more training instances** than *atoms in the observable universe* for **training instances** to be within $0.1$ each other on **average**
      - Assuming they were **spread** out **uniformly across all dimensions**.

## Main Approaches for Dimensionality Reduction

### Projection

- In **most real world problems**
  - The *training instances* $\to$ are *not spread* out **uniformly** across **all dimensions**
    - Many are **constant** , with some being **highly corelated** 
- As a *result* $\to$ all **training instances** lie within **or close to** a *much lower dimensional subspace* of the **high dimensional space**
  - Sounds **very abstract**
    - View this **figure** to see a **3D** data set **represented by circles**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211202203131462.png)

- See how **all training instances** lie *close to a plane*
  - This is a **lower dimensional** 2D *subspace* of the **3D** space in a **higher dimension**.
- *Projecting* each **training instance** *perpendicular* onto this **subspace*
  - Represented by **short lines** *connecting the instances to the plane*
    - We obtain the **new 2D dataset** shown below
- This is **reduction** from **3D** $\to$ **2D**
  - The **axes** correspond to **new features** $z_1$ and $z_2$ (*coordinates of the projections on the plane*)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211202203448177.png)

- Projection is **not always** the **ideal approach** to *dimensionality reduction*
  - Many cases $\to$ the **subspaces** may *twist  / turn*
    - Such as in the **swiss roll toy data set**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211202203541998.png)

- Simply **projection** onto some *plane* by *dropping* $x_3$
  - Would **squash layers** together (**left**)
- What you **really want to do** is **unroll** the *swiss roll* to obtain the **right 2D dataset**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211202203645386.png)

### Manifold Learning

- Swiss role is an example of a **2D manifold**
  - A **2D manifold** $\to$ is a **2D shape** that can be **bent / twisted** within some **higher dimensions**
- Generally $\to$ a $d$ **dimensional manifold** is part of an $n$ dimensional space (where $d < n$) 
  - That **locally** resembles a $d$ dimensional **hyperplane**
    - For our **swiss** roll $d=2$ and $n=3$ 
      - Locally resembles some **2D plane** however is **rolled** within the **third dimension**.

---

- *Dimensionality reduction* algorithms work by **modelling** the **manifold** on which **training instances lie**
  - This is **==manifold learning==**. 
    - Relies on the *manifold assumption* or *manifold hypothesis*
      - Holds that **most real world** *high dimensional datasets* lie **close** to a **much lower dimensional manifold**
        - Often *empirically observed*

---

- Once again $\to$ consider $MNIST$ 
  - All **handwritten digit images** have *some* **similarities** 
    - They are made **of connected lines**
      - The **borders of white** $\to$ they are **more or less centered** etc.
- If you **randomly generated images**
  - Only a *small fraction* would **look like handwritten digits**.
    -  The **degrees of freedom** if you attempt to **create a digit** are *much lower* than the **degrees of freedom** you have if you were **allowed** to **generate** *any image you wanted*
      - These **constraints** tend **to squeeze** the *dataset* into a **lower dimensional manifold**.

---

- The **manifold assumption** $\to$ often accompanied by **another implicit assumption**
  - That the *task* (be that regression or classification) 
    - Will be **simpler** if expressed in the **lower dimensional space** of the *manifold*
- Example $\to$ in the **top row** of figure below

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211202205128899.png)

- Roll split into **two classes** 
  - For **3d Space** in *top left* $\to$ the **decision boundary** is *fairly complex* 
  - But the **2D unrolled manifold** on the *right* is a **straight line**

- This assumption **does not always hold**
  - In the *bottom row* $\to$ the decision boundary is **located** at $x_1=5$ 
    - This looks **simple in 3D** as a *plane* but in **unrolled manner** its **more complex**
      - Collection of **four independent line segments**.
- Thus $\to$ if you *reduce* the **dimensionality** of the **training set** *before* training a **model**
  - It often **speeds up training** but may **not always** lead to a **better or simpler solution**
    - Depends on the **dataset**.

## Principle Component Analysis

- First **identifies** the *hyperplane* that lies **closest to the data**
  - Then just **projects** the *data onto it*

### Preserving variance

- Before projecting the **training set** to a **lower dimensional hyperplane**
  - Must *select* the **correct hyperplane**
- Example $\to$ a simple **2D** dataset represented below
  - With **three different axes** (*each one* being some *one dimensional hyperplane*) 
- On the **right** $\to$ is the **result** of the **projections** of the *dataset* onto *each axes*
  - The **projection** onto the **solid line** $\to$ **preserves** the *most variance*
  - The **projection** onto the **dotted line** $\to$ **preserves** the *least variance*
  - The **dashed** $\to$ *intermediate variance* 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211202212357823.png)

- You select the **axis** that has the **preserves** the **higher variance**
  - As this *likely loses less information*. 
- Alternative justification of a choice
  - Is the **axis** that *minimises* the **mean squared distance** between the *original dataset* and its **projection** onto the **axis**
    - This is the core idea of **PCA**.

### Principle Components

- **PCA** $\to$ identifies the **axis** that accounts for the **largest ammount of variance** in the *original training set*
  - Above $\to$ its the **solid line**.
- Finds also some **second axis** that is **orthogonal** to the *first one*
  - This accounts for the **largest remaining variance** 
- For this **2D example**.
  - There is **no choice** $\to$ its the **dotted line** to be the **other axis**
- In **higher dimensions**
  - A **third axis** is *located* that is **orthogonal again** to each of the **orthogonal axis**
  - fourth, fifth etc....
- The number of **axis** they are depends on the **number of dimensions** in the **dataset**.

---

- The **unit vector** that defines the $i^{th}$ *axis* = $i^{th}$ **principle component** (PC)
  - In **figure above**:
    - $1^{st}$ PC = $c_1$ 
    - $2^{nd}$ PC = $c_2$ 
- In the **figure 8.2 (start of this chapter)**
  - The first **two PCs** are represented by the **orthogonal arrows** in the **plane**
  - The **third PC** would be **orthogonal to the plane**  (up or down direction)

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211202213836097.png)

---

- How to **locate the principle components** of the **training set**
  - There is some **standard matrix factorization** technique called a **Singular value decomposition** ($SVD$) 
    - That is able to **decompose** the *training set matrix* $X$ into the **matrix multiplication** of *three matrices* $U \Sigma V^T$ 
      - Where $V$ contains **all the principle components** that we are *looking for*.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211202214102820.png)

- Following **python code** uses *numpy* `svd()` to obtain **principle components** of the **training set**
  - Then we **extract** the **first two** *principle components*.

```python
X_centered = X - X.mean(axis=0)
U, s, Vt = np.linalg.svd(X_centered)
c1 = Vt.T[:, 0]
c2 = Vt.T[:, 1]
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211202214207558.png)

---

### Projecting down to $d$ dimensions

- Once **identified** to all the **==principle components==**
  - Can *reduce* the **dimensionality** of the *dataset* down to $d$ dimensions by **projecting** it *onto* the *hyperplane*
    - That is **defined** by $d$ *principle components* 
- Selecting **this hyperplane** $\to$ ensures the **projection** will *preserver* as **much variance** as *possible*
  - Example $\to$ in the **figure of plane above in projection section** $\to$ the $3D$ dataset $\to$ *projected* down to the $2D$ plane defined by the **first two principle components**
    - Preserves a **larger part** of the **datasets** *variance*.
      - As a *result* $\to$ the $2D$ *projection* looks **similar** to the **3D dataset**.

---

- To **project** the *training set* $\to$ onto the **hyperplane**
  - You can *simply* just **compute the matrix multiplication**  of the *training* set matrix $X$ by $W_d$ $\to$ defined as the **matrix** containing the *first* $d$ **principal components** 
    - The matrix that is **composed** of the **first** $d$ *columns* of $V$ as shown **below**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204170152988.png)

- Python **code** $\to$ below *projects* the **training set** onto the **plane** defined by the **first two principle components**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204170250482.png)

- This is the **reduction** of **dimensionality** that we *wish to reach*.

### Using sklearn

- sklearn $PCA$ class **implements** $PCA$ using **SVD** decomposition just **like before**.
  - Following code applies $PCA$ to **reduce the dimensionality** of the **dataset** down to **two dimensions** 
    - It *automatically* takes care **of centering the data**.

```python
from sklearn.decomposition import PCA

pca = PCA(n_components = 2)

X2D = pca.fit_transform(X)
```

- After **fitting** the $PCA$ **transformer** to the **dataset**
  - You may access the **principle components** using the `components_` variable 
    - This contains the $PC$ as **horizontal vectors** $\to$ therefore *for example* the **first PC** is *equivalent* to `pca.components.T[:,0]` 

### Explained Variance Ratio

- A further **useful** piece of **information** is the *==explained variance ratio==* of each **principle component**
  - Accessed via the `explained_variance_ratio` variable.
- This **indicates** the **proportion** of the *datasets* variance that **lies** along the **axis** of **each principle component**
  - Example $\to$ look at the **explained variance ratio** of the **first two components** of the **3D dataset** (*plane in projection fig 8.2*)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204171517237.png)

- Tells us that $84.2\%$ of the **datasets** variance *lies* along the **first axis**
  - And $14.6\%$ along the **second axis**
- This leaves **less** than $1.2\%$ for the **third axis**
  - Therefore its **presumed** to have **little information**.

### Choosing the right number of dimensions

- Rather than **arbitrarily** *choosing the number* of **dimensions** to *reduce down to*
  - Generally **preferable** to **choose** the **number of dimensions** that add to a *sufficiently* **larger portion** of the *variance* (such as $95\%$)
- Unless you are **reducing** for **data visualisation** reasons
  - In this case $\to$ you **wish** to **reduce** to $2$ or $3$. 

---

- Following Code computes $PCA$ with **no reduction in dimensionality** 
  - Then **computes** the *number* of *dimensions* required to **preserve** $95\%$ of the **training set** *variance*

```python
pca = PCA()
pca.fit(X_train)
cumsum = np.cumsum(pca.explained_variance_ratio_)
d = np.argmax(cumsum >= 0.95) + 1
```

- Could then set `n_components = d` then run $PCA$ again.
  -  There is a **better alternative** $\to$ rather than **specifying** the *number of principle components* you wish to **preserve**
    - You can set `n_components` to be a **float** between $0.0$ and $1.0$ indicating the **ratio of variance** that *you wish to preserve*.

```python
pca = PCA(n_components=0.95)
X_reduced = pca.fit_transform(X_train)
```

- Alternative **option** $\to$ plot the **explained variance** as a **function** of the *number of dimensions* (*just plot cumsum*)
  - There will be an **elbow** in the **curve**
    - Where the **explained variance** *stops growing* *fast* 
- Think of the **intrinsic dimensionality** of the **dataset** 
  - For this **case** $\to$ see that **reducing** the *dimensionality* down to **about** $100$ dimensions would **lose** *too much explained variance*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204173842307.png)

### PCA for compression

- Obviously after **dimensionality reduction**
  - The *training set* $\to$ takes up **much less space**.
- Applying $PCA$ $\to$ $MNIST$ dataset while **preserving** $95\%$ of its **variance**
  - Should **find** that **each instance** has **just over 150 features**
    - Rather than **original** $784$ *features*
- While **most of the variance is preserved** $\to$ but with $< 20\%$ of **its original size**
  - This is a **reasonable compression ratio** 
    - Can see how it **speeds** up the **classification algorithm greatly** such as $SVM$ classifer.

---

- Possible to **decompress** the *reduced dataset* $\to$ back to the $784$ dimensions
  - Just apply the **inverse** *transformation* of the $PCA$ *projection*
- This **will not return the original data** as the *projection* has **lost some information** (the $5\%$ variance)
  - Its likely **to be very close** to the **original data**
    - The **mean squared distance** between the **original data** and the **reconstructed data** (*compressed / decompressed*) is the **==reconstruction error==** 
- Example $\to$ the **following code** $\to$ *compresses* the $MNIST$ the **dataset** to $154$ *dimensions*
  - Then uses the `inverse_transform()` method to **decompress** it **back** to $784$ dimensions
- Figure below shows a **few digits** from the **original training set** (*on the left*)
  - Along with the **compression** and **decompression**
    - There is a **slight image quality loss** but *mostly intact*

```python
pca = PCA(n_components = 154)
X_reduced = pca.fit_transform(X_train)
X_recovered = pca.inverse_transform(X_reduced)
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204175215430.png)

- Equation of the **inverse transformation** shown in **equation below**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204180007485.png)

### Randomized PCA

- Setting the `svd_solver` **hyperparameter** to the *randomized*
  - **sklearn** $\to$ uses a **stochastic algorithm** called **==randomized PCA==** to find an **approximation** of the *first* $d$ **principle components**
    - The **complexity** is $O(m \times d^2) + O(d^3)$ rather than $O(m \times n^2) + O(n^3)$ for **full** $SVD$ *approach*
      - Therefore its **dramatically faster** than *full* $SVD$ when $d$ is **much smaller** than $n$ 

```python
rnd_pca = PCA(n_components=154, svd_solver="randomized")
X_reduced = rnd_pca.fit_transform(X_train)
```

- As a **default** $\to$ the `svd_solver` set to **auto**
  - This means **sklearn** *automatically* uses this **random PCA method** if $m$ or $n$ is **greater** than $500$ 
  - Alongside $d$ being than $80\%$ of $m$ or $n$ 

- Otherwise just uses **full SVD approach**
  - To *force* *sklearn* $\to$ to **use full SVD** $\to$ can set `svd_solver` to **full**.

### Incremental PCA

- A **problem** with the **preceding implementations** of $PCA$ 
  - They require the **whole training set** to be *within memory* in order for the **algorithm** to **run**
- **==Incremental PCA==** ($IPCA$) algorithms have **been made**
  - Split the **training** set into **mini batches** and **feed** an $IPCA$ *algorithm* one **mini batch** at **time**
- This is *useful* for **large training sets** 
  - Also for **applying** $PCA$ *online*
    - Such as the **fly as the new instances arrives**.

---

- Following code **splits** the $MNIST$ dataset $\to$ **100 mini batches**
  - Uses numpy `array_split()` 
    - Then feeds this to **sklearns** `IncrementalPCA` class to **reduce dimensionality** of the $MNIST$ dataset $\to$ **154 dimensions**, just as *before*.
- Must call `partial_fit()` method with **each mini batch** rather than the `fit()` method with the **whole training set**.

```python
from sklearn.decomposition import IncrementalPCA

n_batches = 100

inc_pca = IncrementalPCA(n_components=154)

for X_batch in np.array_split(X_train, n_batches):
    inc_pca.partial_fit(X_batch)
    X_reduced = inc_pca.transform(X_train)
```

- Alternatively use `Numpy` **memmap class**
  - Allows you to **manipulate** a **large array** that is **stored in a binary file** on **disk** as if it were **entirely memory**
    - The *class* loads *only* the **data** it **needs in memory** when it *requires it* since `IncrementalPCA` class uses **only** a **small part** of the **array** at some time
      - The **memory usage remains** *in control*
- Enables us to be able to **call** `fit()` method
  - As you may **view** in the **following code**:

```python
X_mm = np.memmap(filename, dtype="float32", mode="readonly", shape=(m, n))

batch_size = m // n_batches

inc_pca = IncrementalPCA(n_components=154, batch_size=batch_size)

inc_pca.fit(X_mm)
```

## Kernel PCA

- Chapter on **SVM**
  - Talked about **kernel trick** $\to$ a **mathematical technique** that *implicitly maps* instances 
    - To some **very high dimensional space** refereed to as **the feature space**.
- *Enabling* $\to$ *nonlinear classification* and *regression* with **support vector machines**
  - Recall that a **linear decision boundary** in the **high dimensional feature space**
    - Corresponds to a **complex nonlinear decision boundary** within the **original space**.

---

- It *turns* out that **same trick** can be *applied* to $PCA$ making it **possible** to **perform** *complex nonlinear projections*
  - For means of **dimensionality reduction**
    - This is the **kernel PCA**.
- Very good at **preserving clusters of instances** *after projection*
  - Sometimes **even unrolling datasets** that *lie close* to a **twisted manifold**.

---

- Example $\to$ the **following code** uses `sklearn` `KernelPCA` class to **perform** `kPCA` with an `RBF` kernel

```python
from sklearn.decomposition import KernelPCA
rbf_pca = KernelPCA(n_components = 2, kernel="rbf", gamma=0.04)
X_reduced = rbf_pca.fit_transform(X)
```

- Figure below **shows** the **swiss roll**
  - Reduced to **two dimensions** using a **linear kernel** $\to$ *equivalent* to just using the $PCA$ class
- An **RBF kernel** and **sigmoid kernel** 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204182624110.png)

## Selecting a kernel and tuning hyperparameters

- As $KPCA$ is an **unsupervised learning algorithm** 
  - There is **no obvious performance measure** that helps us select the **best kernel** and **hyperparameter values**.
- However **dimensionality reduction** $\to$ is *often* a *preparation step* for a **supervised learning task**
  - Such as **classification**
    - Thus just use a **grid search** to select the **kernel** and **hyperparameters** that *lead* to the **best performance** on *that task*.
- Example $\to$ following **code** *creates* a **two step pipeline**
  1. First **reduces dimensionality** to **2 dimensions** using **kPCA**
  2. Applies **logistic regression** for **classification.**
- Then uses `GridSearchCV` to find the **best kernel** and **gamma value** for the $kPCA$ in order to **get the best classification** accuracy at the **end of the pipeline**

```python
from sklearn.model_selection import GridSearchCV
from sklearn.linear_model import LogisticRegression
from sklearn.pipeline import Pipeline

clf = Pipeline([
	("kpca", KernelPCA(n_components=2)),
	("log_reg", LogisticRegression())
	])

param_grid = [{
		"kpca__gamma": np.linspace(0.03, 0.05, 10),
		"kpca__kernel": ["rbf", "sigmoid"]
	}]

grid_search = GridSearchCV(clf, param_grid, cv=3)
grid_search.fit(X, y)
```

- The **best kernel / hyperparameters** are *available* through `best_params_` variable

```python
>>> print(grid_search.best_params_)
{'kpca__gamma': 0.043333333333333335, 'kpca__kernel': 'rbf'}
```

- Alternative approach $\to$ entirely **unsupervised** 
  - To *select* the **kernel** and **hyperparameters** that *yield* the *lowest reconstruction error*.
    - However **reconstruction** is *not as easy* as **with linear PCA**

- Figure 8-11 $\to$ shows the **original roll** $3D$ dataset (*top left*) and the **resulting** $2D$ *dataset* after $kPCA$ is applied using an **RBF** *kernel* (*top right*).

---

- Thanks to the **kernel trick** $\to$ this is **mathematically equivalent** to *mapping* the *training* set to an **infinite dimensional feature space** (*bottom right*)
  - Using the *feature map* $\phi$  
    - Then **projecting** the *transformed linear training set* down to **2D** using **linear PCA**
- Notice that if **we could invert** the *linear PCA* 
  - For some **given instance** in the **reduced space**
    - The **reconstructed** point would *lie* in **feature space** and **not original space** (like the single one represented by $x$ in the **diagram**)

- Since the **feature space** has **infinite dimensional**
  - We can **not compute the reconstructed point**
    - Therefore we **cannot compute** the *true reconstruction error*
- Fortunately $\to$ its **possible** to *find* a **point** in the **original space** that *maps*  close to the **reconstructed** point
  - This is called the **reconstruction** *pre image*
    - Once there is a *pre image* $\to$ can **measure** its **squared distance** to the **original instance** 
      - Then *select* the **kernel and hyperparameters** that *minimize* the **reconstruction** *pre-image error*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204190559298.png)

- You may be **wondering** how to *perform* this *reconstruction*
  - One solution $\to$ train a **supervised regression model**  with the **projected instances**
    - As the **training set** and the **original instances** as the *targets*.
- *sklearn* $\to$ does this **automatically** if *you set* `fit_inverse_transform= True` 
  - Shown in the **following code**:

```python
rbf_pca = KernelPCA (
    n_components = 2,
    kernel="rbf",
    gamma=0.0433,
	fit_inverse_transform=True
)

X_reduced = rbf_pca.fit_transform(X)
X_preimage = rbf_pca.inverse_transform(X_reduced)
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204191007317.png)

- Then compute the **reconstruction pre image error**

```python
>>> from sklearn.metrics import mean_squared_error
>>> mean_squared_error(X, X_preimage)
32.786308795766132
```

- Now can use **grid search** with **cross validation** to find the **kernel / hyperparameters** that **minimise** the **pre image reconstruction error**

### LLE

- **==Locally Linear Embedding==** (*LLE*) 
  - Powerful *nonlinear dimensionality reduction* $NLDR$ technique.
- It is a **manifold learning technique** that *does not rely on projections* like the **previous algorithms**
  - Works by **first measuring** how *each training* instance *linearly relates* to its **closest neighbours**
    - Then looking for a **low dimensional representation** of the *training set* where these **local relationships** are **best preserved** (*more detail below*)
- Makes it **good** at **unrolling** the *twisting manifolds*
  - Especially when there **is not too much noise**.

---

- Following code uses *sklearn* `LocallyLinearEmbedding` class to **unroll** the **swiss roll**.
  - Resulting **2D dataset** is *shown* in figure below 
    - The **swiss roll** is **completely unrolled** and the *distances* between **instances** are *locally preserved well*
- But **distances** are **not preserved** on the **larger scale**
  - The *left part* of the **unrolled swiss roll** is *stretched* while the *right part* is **squeezed**
    - nevertheless $\to$ $LLE$ performs a **good manifold model**

```python
from sklearn.manifold import LocallyLinearEmbedding
lle = LocallyLinearEmbedding(n_components=2, n_neighbors=10)
X_reduced = lle.fit_transform(X)
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204192819303.png)

- Process how $LLE$ works
  1. For each **training** $x^{(i)}$ 
  2. Algorithm identifies $k$ **closest neighbours** (*above* $k=10$) 
  3. Tries to **reconstruct** $x^{(i)}$ as a **linear function** of these *neighbours* 
     - Specifically $\to$ finds weights $w_{i,j}$ such that **squared distance** between $x^{(i)}$ and $\Sigma^m_{j=1}w_{i,j}x^{(j)}$ 
       - Is **as small as possible** $w_{i,j} = 0$ if $x^{(j)}$ is **not** one of the $k$ *closest neighbours* of $x^{(i)}$ 
  4. Thus the **first step** of $LLE$ is the **constrained optimization** *problems* described in equation below.
     - Where $W$ is the **weight matrix** containing all the **weights** $w_{i,j}$ 
  5. The **second constraint** just *normalizes* the **weights** for **each training instance** $x^{(i)}$  

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204193938740.png)

6. After this **step**, The **weight** matrix $\hat{W}$ containing the **weights** $\hat{w}_{i,j}$ 
   - Encodes the **linear relationship** between **training instances**
7. Must now **map** the *training instances* to $d$ *dimensional space* where $d < n$ 
   - Must **preserving** these *local relationships* as **much as possible**
     - If $z^{(i)}$ is the image of $x^{(i)}$ in the $d$ *dimensional space* 
       - Then we want the **squared distance** between $z^{(i)}$ and $\Sigma^m_{j=1}\hat{w}_{i,j}z^{(i)}$ to be **as small as possible**.
   - Idea leads to the **unconstrained optimisation problem** described *equation below* 
     - Looks **similar to the first step** 
       - Rather than *keeping* the **instances fixed** and finding the **optimal weights**
         - We perform the **reverse** $\to$ *keeping* the **weights fixed** and **finding the optimal position** of the **instances** *images* in the *low dimensional space*
   - Note $Z$ is the **matrix containing** all $z^{(i)}$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204195857222.png)

- Sklearn LLE implementation has the **following computational complexity**
  - $O(m \log(m)n \ \log(k))$ for finding the $k$ *nearest neighbours* 
  - $O(mnk^3)$ for **optimizing** the **weights** and $O(dm^2)$ for construction the **low dimensional representations**
- Unfortunately $\to$ $m^2$ in the **final term** means it scale **poorly for large datasets**.

### Other dimensionality reduction techniques

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204204330081.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204204339001.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204204349575.png)

