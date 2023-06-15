# Decision Trees

- Decision trees can perform **classification** and **regression**
  - Can fit **complex datasets**.
- Chapter 2 / coursework 1 $\to$ we trained a `DecisionTreeRegressor` fitting it **perfectly**.
- They are **fundamental components** of *Random Forests*.

## Training and Visualizing a Decision Tree

- Build one first and look at how it **makes predictions**
  - Code trains a `DecisionTreeClassifier` on the **iris dataset**

```python
from sklearn.datasets import load_iris
from sklearn.tree import DecisionTreeClassifier

iris = load_iris()
X = iris.data[:, 2:] # petal length and width
y = iris.target
tree_clf = DecisionTreeClassifier(max_depth=2)
tree_clf.fit(X, y)
```

- Can visualise the **trained** **decision tree** by first using the `export_graphviz()` method to **output a graph definition** file called the *iris_tree.dot*

```python
from sklearn.tree import export_graphviz
export_graphviz(
        tree_clf,
        out_file=image_path("iris_tree.dot"),
        feature_names=iris.feature_names[2:],
        class_names=iris.target_names,
        rounded=True,
        filled=True
)
```

- Then you **convert** this  `.dot` file to a **variety** of **formats** such as $PDF$ or $PNG$ using the `dot` *command line tool* from the `graphviz` package.
  - This **command line** *converts* the `.dot` file to a `.png` *image file*

```bash
$ dot -Tpng iris_tree.dot -o iris_tree.png
```

- Your **first decision tree**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124225107739.png)

## Making Predictions

- Suppose you **find** some *iris flower* and you wish to **classify**
  - Start at the *root node* (*depth* $0$ at the **top**)
    -  This *node* will **ask** whether the **flowers petal length** is *smaller* than $2.45cm$ 
      -  If so $\to$ move **down** to the **left child left**
        - This case its a **leaf nodes** at *depth 1*
          - Therefore asks **no questions**.
- Simply look at *predicted class* that the **leaf node makes** which is `setosa` 

---

- Say you find one with length **greater than 2.45cm** 
  - Move to the **right child node** at *depth 1 right*
    - This is **not a leaf node** Therefore **asks another question** 
      - If petal width less than 1.75 
        - Most likely a **virginica** else **versicolor** etc.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124230216329.png)

- A *nodes*  `samples` **attribute** *count* how **many training instances** it *applies to*
  - Example $\to$ $100$ training instances have **petal length** *greater* than *2.45cm*.
- A nodes `values` **attribute** tells you  **how many training instances** of *each class* this *node this applies to*
  - Bottom right $\to$ $0$ to **iris setosa** and $1$ for **versicolor** and $45$ for **virginica**
- A *nodes* `gini` attribute measures its *impurity*
  - A node **pure** at *gini = 0.6* if **all training instances** it applies to **belong to same class**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124230709929.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124230813348.png)

> You **square the gini impurity** as to **calculate** its relative **ratio** as for example [0,0.5,0.5], this is clearly a 0.5 impure
>
> computed as 1 - (0.5^2 + 0.5^2) = (1 - 2(0.25)) = 0.5, 
>
> intuitively its like saying take the **probability** of this **class occurring** and **multiply** it by the **magnitude** that the **class holds** or the overall number of **relative states**
>
![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125110647677.png)
>
> - Think of the probability as **squares**, and this explains the **squaring reason** for calculating the **gini**
> - we take away 1 - sum of all these squares to obtain the **different / black part** which is the **impure value**

- Figure below shows the **decision trees decision boundaries**.

- The *thick vertical line represents* the **decision boundary** of the *root node* (depth = 0)
  - Which is just **petal length** = 2.45

- The **left area is pure**
  - It **cannot be split further**
- The right **is impure**
  - so the **depth - 1** right *node splits* at **petal width** = 1.75 shown by **1.75** 
    - Since the `max_depth` was set to $2$ $\to$ decision tree **stops right there** 
      - If you set `max_depth` to $3$ then **two depth 2 nodes** would *add another decision boundary*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124231303412.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211124233425338.png)

## Estimating Class Probabilities

- A **decision tree** can also *estimate the probability* that some **instance** belongs to **some class** $k$ 
  - *Traverses* the tree to locate the **leaf node** for the **instance** 
    - Returns the *ratio* of **training instances** of class $k$ in this node which is **stored in the node** `value` data array.

```python
>>> tree_clf.predict_proba([[5, 1.5]])
array([[0. , 0.90740741, 0.09259259]])

>>> tree_clf.predict([[5, 1.5]])
array([1])
```

- Estimated **probabilities** are *identical* anywhere in the **bottom right rectangle**

## CART Training Algorithm.

- *Scikit learn* uses the **classification and regression tree** $CART$ *algorithm* to *train* **decision trees**.
  - Also referred to as **growing trees** 

- Algorithm
  - Splits the **training** in *two subsets* using a **single feature** $k$ and a **threshold** $t_k$ (petal length <= 2.45) 
    -  How does it select to $k$ and $t_k$ 
      - Searches for the **pair** $(k,t_k)$ to produce the **purest subsets** (*weighted by size*)
        - The **cost function** to *minimize* this **algorithm**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125095527210.png)

- Once it has **succesfully** *split* the *training* set **in two**
  - It *splits* the *subsets* using the **same logic** $\to$ then the **subset** and *so on* *recursively*.
    - It **stops recursing** once it reaches some **max depth** defined by the `max_depth` *hyperparameter* 
      -  Or if it **cannot locate** a *split* that **reduces impurity**.
- A *few other hyperparameters* may **control stopping conditions**
  - mentioned in **hyperparameters section**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125100223523.png)

## Computational Complexity

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125100405762.png)

## Gini Impurity or Entropy

- The **gini impurity** used as a **default** 
  - You may select the *entropy* **impurity** *measure* instead by **setting** the **criterion** *hyperparameter* to `"entropy"` 
- The idea of **entropy** has *origins* in **thermodynamics** as a **measure** of **molecular disorder**.
  - Entropy *approaches* **zero** when the **molecules** are *still / well ordered*
    - Spreads into the idea of **information theory** 
      - This measures the **average information** *content* of *message*
        - Entropy **is zero** when **all messages are identical**

- For ML $\to$ used as an **impurity measure** *often*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125101417872.png)

- Entropy is **zero** when there is a **single class** presen.
  - Equation below shows the **definition** of the **entropy** of the $i^{th}$ node 
    - Example $\to$ the **depth 2** *left node* in **diagram above figure 6.1** has an entropy

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125101617011.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125101916345.png)

https://www.quora.com/Why-is-there-a-logarithmic-function-in-the-entropy-formula

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125104204335.png)



> -- self notes --- 
>
> Negate to obtain the loss value of this summation.

>  So the entropy loss calculates the **percentage / ratio** of **some class** multiplied by the **total number of bits of information which is equivalent ** to the **number of states** in *total* 

> Taking this as a **negated sum** will **negate the log** to give us the **probability** in the log rather than storing teh **number of bits**

> The uncertainty reduction is the inverse of the events probability thus $\log_2{\frac{1}{x}} = -\log_2{x}$ 
>
> where $x$ in this case is the **ratio** or **probability** of a **class** and , but its **directly linked** to **log** of the **reduction** as in how much more **simplified the problem** got after **splitting** the **values** on **known information** such as for **decision trees**.  
>
> information or bits is to do with how much does the **knowledge of some probability** actually help us in **finding the impurity** of the **overall value** and this must be **scaled** by the **probability** to calculate the **entropy** 
>
> which is just the **overall disorder** or **uncertainty** of **class distribution**.
>
> https://www.youtube.com/watch?v=YtebGVx-Fxw - Just watch this it makes sense
>
> Summary
>
![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125113425734.png)
>
> - From this log_2(1/px) = -log_2(ratio_classes) and probability associated
> - so the entropy is clearly the probability of some surprise of this happening, which is higher for lower probabilities of course
>
![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125113622467.png)

- So should you **use gini impurity** or **entropy**
  - This does **not make much difference** as they lead to **similar trees**
    - **Gini is slightly faster** 
      - Gini attempts to **isolate** the **most frequency class** in its **own branch of the tree**
        - Whereas **entropy** tends to produce **slightly more balanced trees**.

## Regularization Hyperparameters

- Decision trees make **little assumptions** about the **training data**
  - Opposed to *linear models* which *assume data is linear of course*
    - If **left unconstrained** $\to$ the **tree structure** will *adapt itself to the training data* 
      - Thus **fitting very closely** and *most likely overfitting*
        - This is a **==nonparametric model==** as the **number of parameters** is *not determined* before the **training process** (*not that is doesnt have any parameters*) 
          - Therefore the **model** can freely **stick to the data closely**
- Contrastly, a **==parametric model==** such as a **linear model** has some *predetermined* number of **parameters** 
  - Thus its **degree of freedom** becomes *limited* at a bonus of **less overfitting risk** and *cost* of **possible underfitting**.

---

- To **avoid overfitting** the *training data*
  - Must **restrict** the *decision tree freedom* during **training**
    - This is as defined before called **regularization** 
- The **hyperparameters** depend on the **algorithm in use**
  - Often can at *least restrict* the **maximum depth** of the **decision tree**
- In *sklearn* this is controlled via `max_depth` *hyperparameter*
  - Default set to `None` which means **unlimited**
- *Reducing* the `max_depth` will **regularize the model** to *prevent overfitting risk*

---

- The `DecisionTreeClassifier` class has *few other parameters* to *restrict* the **shape of the decision tree**
  - `min_samples_split` - **minimum** number of samples a **node** has **before it can split**
  - `min_samples_leaf` - **minimum** number of samples a **leaf node** must have.
  - `min_weight_fraction_leaf` - same as `min_samples_leaf`  but expressed as a **fraction of total number of weighted instances**
  - `max_leaf_nodes` - **maximum** number of **leaf nodes**
  - `max_features` - **maximum** number of features that are **evaluated** for **splitting** at *each node*.

- Increasing the `min_*` hyperparameters or reducing the `max_*` hyperparameters will **regularize the model**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125120235585.png)

https://www.youtube.com/watch?v=WXPBoFDqNVk - chi squared test 

> Summary, looks at expected value and observed value, and calculates a value of difference out of the expected value 
>
> if this value calculated from summing this result (sum of squares difference / expected) is less than a threshold defined
>
> we accept the value
>
> the null hypothesis being is accepted if this threshold is not met and rejected if exceed

- Figure below shows **two decision trees** *trained* on the *moons dataset*

  - On the **left** $\to$ the *decision* *tree* is **trained** with the **default hyperparameters** 
    - As in **no restrictions**.

  - On the **right decision tree** its trained with `min_samples_leaf =4` 
    - Its **quite obvious** that *the model* on the **left** is **overfitting**
      - And the *model* on the **right will probably generalize better**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125184551010.png)

## Regression

- Decision **trees** are *also capable* of **performing** of **regression tasks**
  - lets **build** a **regression** *tree* using *scikit-learn* `DecisionTreeRegressor` class
    - *Training* it *on a noisy quadratic dataset* with `max_depth=2`:

```python
from sklearn.tree import DecisionTreeRegressor
tree_reg = DecisionTreeRegressor(max_depth=2)
tree_reg.fit(X, y)
```

- Resulting **tree**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125190143410.png)

- This **tree** looks **similar** to the **classification** you *built earlier*
  - The **main difference** is that *instead* of **predicting a class in each node**.
    - It instead a **predicts a value**.
- Example $\to$ say you want to make a **prediction** for some **new instance** with $x_1 = 0.6$ 
  - **Traverse** the *tree* starting at the **root** 
    - Eventually reach the **leaf node** that *predicts* `value = 0.1106`.
      - This **prediction** is just the **average target value** of the $110$ *training instances* associated to the **leaf node**
- This prediction **results** in the **Mean squared error** equivalent to $0.0151$ over **110 instances**.

---

- This *models* predictions are **represented** on the *left of figure below*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125190850886.png)

- Setting `max_depth = 3` 
  - You obtain the **predictions represented** on the *right*.
    - Notice that the **predicted value** for *each region* is always the **average target value** of the **instances** in said region.
- The **algorithm** $\to$ *splits* each region in a way to **make most training instances** as *close* to the **predicted value** as they can.

---

- The $CART$ algorithm works **mostly the same** *way as earlier*
  - Instead of **trying** to **split** the training set to **minimise impurity**
    - Tries to split to **minimise** $MSE$ 
      - Equation below shows the **cost function**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125191232095.png)

https://arifromadhan19.medium.com/regrssion-in-decision-tree-a-step-by-step-cart-classification-and-regression-tree-196c6ac9711e

- Just like **classification tasks**
  - Decision trees are **prone to overfitting** when *dealing* with **regression tasks**
- With **no regularization** (*using default hyperparameters*)

  - You obtain **predictions** of the *left* thus **severely overfitting**

- Just setting `min_samples_leaf = 10` results in **a much more reasonable model**
  - Represented on the **right**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125192259292.png)

## Instability

- There are **a few limitations**
  - They clearly favour the **orthogonal boundaries** for **decision**
    - This makes them **sensitive** to *training sets* that involve **rotation**.
- Figure below shows a **linearly separable dataset** 
  - On the *left* $\to$ a **decision tree** can *split it easily*
  - On the *right* $\to$ after **rotation** by $45$ degrees $\to$ it looks **convoluted** for no reason.
- Both fit the **training set perfectly** 
  - Likely the **model on the right** will *not generalise as well*
    - We can limit this problem to use **PCA**
      - Which often results in a **better orientation** of the **training data**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125193024198.png)

- In general they are **sensitive** to **small variations** to the **training data**
  - Removing the **widest iris versicolor** from the *training set*
    - Then *train a new decision tree* 
      - May obtain the **model below**.
- It looks **very different** from the **previous** shown above in **figure 6.2**
  - Since the **training algorithm** used by **sklearn** is *stochastic* 
    - You may **obtain different models** even on the **same training data**
      - unless you set the `random_state` *hyperparameter*.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125193427914.png)

- *Random forests* can **limit this instability** by **averaging predictions** over *many trees*

