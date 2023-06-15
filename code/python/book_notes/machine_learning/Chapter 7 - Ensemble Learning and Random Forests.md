# Ensemble learning and Random forests

- Ask a question to **thousands** of **random people**
  - You would **aggregate** their *answers*
- Find that this answer in itself is **better** than the **expert answer**
  - This is the *wisdom of the crowd*
- If you **aggregate** the *predictions* of a **group of predictors**
  - Such as the **classifiers / regressors**
    - Obtain **better predictions** 
- A *group of predictors* is an **==ensemble==** 
  - This is referred to as **==ensemble learning==** 
    - And an algorithm to implement it is **==Ensemble method==** 

---

- Training a **group** of *decision tree classifiers* 
  - Each on a *different* **random** subset of the **training set** 
- To obtain **predictions** 
  - You just *obtain* the *predictions* of **all individual trees**
    - Then *predict the class* with the **most votes**
      - This is an **ensemble** of **decision trees** called a **==Random Forest==**.

---

- Often use **ensemble methods** near the **end** of an **ML project** 
  - To combine into a **better predictor** 
    - The **best solutions** for ML are often **ensemble methods**.

---

## Voting Classifiers

- Say you have trained a **few classifiers**
  - Each one has around $80\%$ accuracy 
    - May have **logistic regression classifier** , **SVM classifier**, **Random Forest classifier**, **K nearest neighbours classifier** and perhaps more

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125195032743.png)

- A **simple way** to create a **better** classifier is to **aggregate the predictions** of *each classifier*
  - Then *predict* the *class* that **gets the most votes**
    - This **majority** vote *classifier* is called the ***hard voting classifier***. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125195359991.png)

- This **voting classifier** often achieves a **higher accuracy** than the *best classifier* in the **ensemble**.
  - Even if every classifier is a **weak learner** (*near random*)
    - The *ensemble* can become a **strong learner** 
      - Provided there is **a sufficient number** of *weak learners* that are **diverse**.

---

> **Analogy to explain**
>
![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125195635193.png)
>
> - Summary 
>   - Tossing a coin with a slightly higher chance than other side a larger ammount of times increases the overall probability over time.
> - The ratio should tend towards the **true expected values**
>
![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125195830564.png)
>
> - Ensemble with 1k classifiers
>   - correct 51% of the time
>     - If you predict the majority voted class
>       - Hope for **75% accuracy** 
>         - Only holds if each classifier is **independent** and *makes uncorrelated errors*
>         - This is impossible if they are **trained on the same data**
>         - Makes the same types of errors, therefore majority votes for the **wrong class** reducing accuracy.

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125200047487.png)

- Code below creates and **trains** a **voting classifier** in *sklearn*
  - Composed of **three diverse classifiers** 

```python
from sklearn.ensemble import RandomForestClassifier
from sklearn.ensemble import VotingClassifier
from sklearn.linear_model import LogisticRegression
from sklearn.svm import SVC
log_clf = LogisticRegression()
rnd_clf = RandomForestClassifier()
svm_clf = SVC()

voting_clf = VotingClassifier(
    estimators=[('lr', log_clf), ('rf', rnd_clf), ('svc', svm_clf)],
    voting='hard'
)
voting_clf.fit(X_train, y_train)
```

- Look at each **classifiers accuracy** on the **test set**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125200758182.png)

- If each classifier can **estimate class probabilities** (meaning they have a `predict_proba()` method)
  - Then you may tell **sklearn** to *predict the class* with the **highest class probability** 
    - This is **averaged** over *all individual classifiers*
      - This is ***soft voting*** 
- Achieves a **higher performance** over *hard voting*
  - As it gives **more weighting** to *highly confident votes*
    - Replace `voting = "hard" ` to `voting = "soft"` 
      - Ensure that **all classifiers** can **estimate class probabilities** 
- This is **not the case** for `SVC` by **default** as you recall
  - Must set its `probabilitiy` **hyperparameter** to `True` 
    - `SVC` uses a **cross validation method** to *estimate class probabilities*.
      - This **slows down training** and adds `predict_proba()` method.
- Modifying code before obtains us $91.2\%$ accuracy

## Bagging and Pasting

- A method of **obtaining** a *diverse set* of *classifiers*
  - Is using very **different training algorithms** 
- Another approach is using the **same training algorithm** for *every predictor*
  - But to **train** on **different random subsets** of the *training set*
    - With *sampling before* using **replacement** $\to$ refer to it as **==bagging==** (*bootstrap aggregating*) 
- When **sampling** is *performed* **without replacement**
  - Often called **==pasting==** 

---

- Both **bagging / pasting** allow *training instances* to be *sampled several times* **across multiple predictors**
  - but only **bagging** enables **training instances** to *be sampled several times* over the **same predictor**
    - This *sampling and training process* is **represented below**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125201951937.png)

- Once **all predictors** are *trained*
  - The **ensemble** may make a **prediction** for a **new instance** by just **aggregating** the *predictions* of **all the predictors**
- The **aggregation** function is often called the *statistical mode* (*most frequent prediction* like *hard voting*)
  -  For **classification** or **average** for **regression**.
    - Each individual **predictor** has a **greater bias** than if it were **trained** on the **original training set** 
- *Aggregation* reduces both **bias / variance**
  - The *net result* is that the *ensemble* has a **similar bias** but **lower variance** than **single predictors** trained on the **original training set**.

---

- Shown in above image
  - Predictors can be **trained in parallel** using various **cores / servers**
- Predictions can be **made in parallel also** 
  - This is one of the **reasons** why **bagging / pasting** are **popular methods**
    - They *scale well*

## Bagging and Pasting in sklearn

- `sklearn` offers a **simple API** for **bagging / pasting** with `BaggingClassifer` class (`BagginRegresion` for *regression*)
  - Code below trains a **500 decision tree ensemble** 
    - Trained on **100 training instances** *randomly sampled* from the **training set** with *replacement*
      - This is **bagging**, set `bootstrap = false` for **pasting**
- The `n_jobs` parameter tells *sklearn* the **number of CPU cores** for *training* and **predictions**
  - Where $-1$ tells *sklearn* to use **all cores available**

```python
from sklearn.ensemble import BaggingClassifier
from sklearn.tree import DecisionTreeClassifier

bag_clf = BaggingClassifier(
	DecisionTreeClassifier(), 
    n_estimators=500,
	max_samples=100,
    bootstrap=True, 
    n_jobs=-1
)

bag_clf.fit(X_train, y_train)
y_pred = bag_clf.predict(X_test)
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125203303328.png)

- Figure below shows **the decision boundary** of a **single decision tree** with **decision boundary** of a **bagging ensemble** of **500 trees** (*previous code*)
  - Both **trained** on the **moons dataset**.
    - As you can **see** $\to$ the **ensembles predictions** are likely to **generalize** *much better* than the **single decision trees prediction**.
- The **ensemble** has a **comparable bias** but **smaller variance**
  - It makes a *roughly* the **same number of errors** on the **training set** but the **decision boundary** is *less irregular*.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125212847393.png)

- Bootstrapping introduces **more diversity** in the **subsets** that *each predictor* is **trained on**
  -  So *bagging ends up* with **slightly higher** *bias* than **pasting**
    - This also *means* that **predictors** end up **being less correlated** 
      - Therefore the **ensembles variance** is *reduced*
- Overall $\to$ **bagging** results in **better models**
  - Explains why its **generally preferred**
    - However *if you have spare time* and **CPU power** you can use **cross validation** to *evaluate* **both bagging / pasting** 
      - Select the *one that work bests*.

## Out of Bag Evaluation

- With **bagging** *some instances* may be *sampled* **several times** for any *given predictor*
  - While some **may never be sampled**
- By default `BaggingClassifier` samples $m$ training instances with **replacements** (`bootstrap = True`)
  - Where $m$ is the **size** of the **training set** 
- This means that only **about** $63\%$ of the **training instances** are *sampled* on *average for each predictor*
  - Remaining $37\%$ of the **training instances** that are *not sampled* are **out of bag** *oob* instances
- Note they are **not the same** $37\%$ for **all predictors**

---

- As a **predictor** *never* sees the **oob instances** during **training**
  - It may be **evaluated** on *these instances* with no need for **seperate validation set**
    - You can **evaluate** the *ensemble itself* by **averaging**  the **oob evaluations** of **each predictor**

---

- In `sklearn` $\to$ can set the `oob_score = True` when creating a `BaggingClassifier` to **request** an *automatic* **oob evaluation** *after training* $\to$ this runs the samples on the current model to evaluate the model before testing.
  - The *following* code **demonstrates** *this*
    - The **resulting evaluation** *score* is *available* **through** the `oob_score_` variable.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125214831586.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125214840969.png)

- According to **this oob evaluation**
  - This `BaggingClassifier` is likely to **achieve** about $90.1\%$ on the **test set**
    -  Lets **verify**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125215004852.png)

- The **oob decision function** for *each training instance* is available via `oob_decision_function` *variable*
  - In **this case** $\to$ since the **base estimator** has a `predict_proba()` 
- The **decision function** $\to$ *returns* the **class probabilities** for *each training instance* 
  - Example $\to$ **oob evaluation** $\to$ estimates the **first training instance** has a $68\%$ probability of **belonging to the positive class**  

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125220224685.png)

## Random Patches and Random Subspaces

- The `BaggingClassifier` class **supports** *sampling features as well*
  - This is **controlled** by *two hyperparameters*
    1. `max_features` 
    2. `bootstrap_features` 
- Work in the **same way** as `max_samples ` and `bootstrap`  
  - But for **feature sampling**
    - Therefore *each predictor* $\to$ will be **trained** on a **random subset** of the *input features*.

---

- This is **particularly useful** when you are **dealing** with *high dimensional inputs*
  - Such as with **images**.
- Sampling both **training instances** and **features** is called the **==random patches method==** 
  - Keeping **all training instances** (`bootstrap = False` and `max_samples = 1.0`)
    - But **sampling features** (`boostrap_features = True` and `max_samples` is **smaller than 1.0**)
      - Called the **==Random Subspaces method==**
- Sampling *features results* in **even more predictor diversity** 
  - Trading a **bit more bias** for **lower variance**. 

## Random Forests

https://www.youtube.com/watch?v=J4Wdy0Wc_xQ -  good overview of everything

- As we have **discussed** a *Random Forest* is an **ensemble** of **Decision trees** 
  - Trained via the **bagging method** (*sometimes pasting*)
    - Typically with `max_samples` set as the **size** of the **training set**.
- Rather than **building** a `BaggingClassifier` and **passing** it a `DecisionTreeClassifier` 
  - Instead use the `RandomForestClassifer` which is **convenient** and **optimized** for use with **decision trees**.
    - Of course `RandomForestRegressor` for **regression tasks**.
- Code below trains a **Random forest classifier** with $500$ trees (*limited at 16 nodes*)
  - Using **all available CPU cores**

```python
from sklearn.ensemble import RandomForestClassifier

rnd_clf = RandomForestClassifier(n_estimators=500, max_leaf_nodes=16, n_jobs=-1)
rnd_clf.fit(X_train, y_train)

y_pred_rf = rnd_clf.predict(X_test)
```

- Some exceptions but **most of the hyperparameters** are shared with `DecisionTreeClassifier` 
  - To control how **trees are grown**
- Alongside of course the **bagging classifiers** parameters for the actual ensemble.

---

- **Random forest** introduces **extra randomness** when *growing trees*
  - Rather than *searching* for the **beat feature** when *node splitting*
    - Searches for the **best feature** among a **random subset of features**
      - Results in a **greater tree diversity** 
        - Trades a *higher bias* for *lower variance*.
          - Generally **yielding overall better model**
- Following `BaggingClassifier` is **equivalent nearly** to the *previous* `RandomForestClassifier` 

```python
bag_clf = BaggingClassifier(
	DecisionTreeClassifier(splitter="random", max_leaf_nodes=16),
	n_estimators=500,
    max_samples=1.0, bootstrap=True,
    n_jobs=-1
)
```

### Extra Trees

- When **growing** a *tree* in **random forest**
  - At *each node* $\to$ only a **random subset** of the *features* is **considered in splitting**

- Possible to make trees **more random** by using **random thresholds** for *each feature* rather than **searching** for *best possible threshold* (like **decision trees do**)

---

- A *forest* of such **extremely random trees**
  - Is called an **==Extremely Randomized Trees==** *ensemble* (or **extra trees** for short)
    - Once **again** $\to$ trades **more bias** for **lower variance** 
- Also makes **extra trees** much **faster** to *train* as opposed to **regular random forests** 
  - Since finding **the best possible threshold** for *each feature* at **every node** is *one of the most time consuming* tasks of **growing a tree**.

---

- Can **create** an *extra trees classifier* using *sklearns* `ExtraTreesClassifier` class
  - its **API** is identical to `RandomForestClassifier` class.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125223239918.png)

### Feature Importance

- Quality of **random forests** is they make it **easy to measure** the *relative importance* of *each feature*
  - *sklearn* $\to$ *measures* a **features importance** by *looking* at how **much of the tree nodes** that use that **feature**
    - Reduce *impurity* on **average** (*across all trees* in the **forest**)
- Precisely
  - Its a **weighted average** where *each nodes weight* is **equal** to the **number of training samples** that are *associated* with it

---

- `sklearn` computes this **score automatically** *for each feature after training*
  - Then **scales** *the result* so the **sum of all importance's**  is equal to $1$. 
    - Access via the `feature_importances_` *variable*.
- Example $\to$ follow code trains a `RandomForestClassifier` on the **iris dataset**
  - And output **each feature importance** 
- Seems the **most important features** are *petal length / width*
  - Sepal much lower.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125224815676.png)

- Similarly training a **random forest classifier** on the $MNIST$ dataset
  - Plot each **each pixel importance**
    - Obtain **this image**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125225016533.png)

- Random forests are useful for obtaining **quick understanding** of the **features that actually matter**
  - In particular if you need to do **feature selection**

## Boosting

- Also called **hypothesis boosting**
  - Refers to an **ensemble method** that can combine **several weak learners** into a **strong learner**
- General idea of **most boosting methods** is to *train predictors sequentially* 
  - Each **trying** to **correct** its **predecessor**
    - There are *many boosting methods available*
      -  But most popular is **==AdaBoost==**  for *adaptive boosting* and **gradient boosting** 

### AdaBoost

https://www.youtube.com/watch?v=LsK-xG1cLYA

- One method for a **new predictor** to *correct its predecessor*
  - Is just **paying more attention** to the *training instances* that the **predecessor underfitted**
    - Results in a **new predictors** set that focuses more on the **hard cases**

---

- to Build An **AdaBoost** classifier
  - A **first base classifier** such as **decision tree** is *trained* and used to **make predictions** on *training set*
- The **relative weight** of *misclassified training instances* is **increased**
- A **second classifier** is *trained* using the **updated weights** and again *makes predictions* on the *training set*
  - Weights are **continuously update as such**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125230058653.png)

- Figure below **shows decision boundaries** of *five consecutive* *predictors* on the **moons dataset**
  - For this example $\to$ each predictor is a **highly regularized** *SVM classifier* with an **RBF kernel**
- *First* classifier obtains **many instances** that are **wrong**
  - Therefore the **weights** are *boosted*
- *Second* classifier thus does a **better job** on these instances etc...

---

- Plot on the **right** *represents* the **same sequence of predictors** except the *learning rate is halved*
  - (The **misclassified** instance weights are **boosted half** as much as **each iteration**)
- As you see $\to$ this **sequential learning** has similarities to **gradient descent**
  - Rather than *tweaking a single predictors* parameters to **minimise the cost function**
    - *AdaBoost* adds  **predictors** to the **ensemble** to gradually make it better.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125230601266.png)

- Once **all predictors are trained**
  - The *ensemble* makes **predictions** similar to that of **bagging / pasting**
    - Except the **predictors** have **various weights** depending on **overall accuracy** on **weighted training set**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125230714037.png)

- Take a **closer look** to the **AdaBoost algorithm**
  - Each instance weight $w^{(i)}$ is **initially set** to $\frac{1}{m}$ 
- A **first predictor** is *trained* and its **weighted error rate** $r_1$ is *computed* on the **training set**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125230854733.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125233116319.png)

> alternatively 
>
> the error rate / total error etc used in weight calculation prevalence
>
> is the sum of current weights stored where the stump misclassified over the total weights of every instance, often one as its normalised.

- The **predictors** weight $\alpha_j$ is computed as so below
  - Where $\eta$  is the **learning rate** *hyperparameter* defaulting at $1$ 
- The **more accurate** said **predictor is** the **higher weight it is**
  - if its just **randomly guessing**
    - Then its weight is **close to** $0$. 
      - If **mostly wrong** then its **negative** (*less accurate than random guessing*)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125233757525.png)

- Classifiers with **accuracy** that are *greater than 0.5* result in **positive weights** for the **classifier**
  - if $w_i > 0$ if $r_j \leq 0.5$  
- Classifier with **0.5 exactly** accuracy is $0$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125235234371.png)

- so you see there is clear **midway point** crossing $0$ 
  - as x increases further the values become more negative indicating more error thus lower weights or decreasing 
- and x decreasing just makes greater weights

---

- Now the **instance weights** are **updated**
  - The **misclassified instances** are **boosted in the weights** as to **focus attention onto them**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125235701746.png)

- So following this if the prediction is the correct as the truth value then keep the weight the same
- otherwise scale the current weight along the exponential of the output weight which ranges from 0 to infinity smoothly to be used as an 

- All **instance weights** are *normalized*
  - As in divided by $\Sigma_{i=1}^mw^{(i)}$ 
- Finally $\to$ some **new predictor** is *trained* using the **updated weights**
  - The process is **repeated** (*new predictor weights computed,  instance weights are updated then another predictor trained*)
- Algorithm ends when the **desired number of predictors** is *reached*
  - Or when a **perfect predictor is formed**

---

- To **make predictions**
  - **Adaboost** just **computes** the **predictions** of *all the predictors* and **weights** them using the **predictor weights** $\alpha_j$ 
    - The **predicted class** is the one that *receives a majority of **weighted votes***

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125233549774.png)

- So the class we select is the sum of the **weights / ammounts of say of each stump** for the **class** in question
- The **class** with a **total sum of weights** that is the **largest** is *selected* as the **classification** for *input* $x$ 
- stumps / predictors used interchangeably

---

- *sklearn* uses a **multiclass version** of **adaboost** called $SAMME$    

---

### Gradient Boosting

- Sequentially **adds predictors** to some **ensemble**
  - Each one $\to$ **corrects** its **predecessor**
    - Rather than **tweaking** the **instance weights** at *each iteration* like *adaboost*
      - Tries to **fit** the **new predictor** to the *residual errors* made by the **previous predictor**

---

- *regression example* $\to$ using **decision trees** as **base predictors**
  - This is **gradient tree boosting** / **gradient boosted regression trees**
    - Fit a `DecisionTreeRegressor` to the **training set**
      - Such as some *noisy quadratic set*

```python
from sklearn.tree import DecisionTreeRegressor

tree_reg1 = DecisionTreeRegressor(max_depth=2)

tree_reg1.fit(X, y)
```

- Then train a **second** one on the **residual errors** (*remaining errors*) made by the **first predictor**

```python
y2 = y - tree_reg1.predict(X)
tree_reg2 = DecisionTreeRegressor(max_depth=2)
tree_reg2.fit(X, y2)
```

- Then a **third**...

```python
y3 = y2 - tree_reg2.predict(X)
tree_reg3 = DecisionTreeRegressor(max_depth=2)
tree_reg3.fit(X, y3)
```

- Now there is an **ensemble containing three trees**
  - Can *make predictions* on a **new instance** simply by **adding** up the **predictions** of all **the trees**.

```python
y_pred = sum(tree.predict(X_new) for tree in (tree_reg1, tree_reg2, tree_reg3))
```

- Figure below:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211202185135382.png)

- Predictions of the trees in the **left column**.
- Ensembles predictions in the **right column**.
- In **first row** $\to$ ensemble has a **single tree**
  - Therefore its **predictions** are the **same**.
- In **second and third** there are **residual error training**
  - The ensembles on the **right** is equivalent to the **summation of these** various lines.
- Gradually gets better with **every residual error calculation**.

---

- Simple method is using `GradientBoostingRegressor` class
  - Similar to `RandomForestRegressor` 
    - Has **hyperparameters** $\to$ control *growth* of the **decision trees** (*max_depth* etc.)
    - Along with **hyperparameters** to control the **ensemble training** 
      - Such as the *number of trees* `n_estimators` 
- Code creates the **same ensemble** as the **previous one**:

```python
from sklearn.ensemble import GradientBoostingRegressor

gbrt = GradientBoostingRegressor(max_depth=2, n_estimators=3, learning_rate=1.0)

gbrt.fit(X, y)
```

- The `learning_rate` will **scale** the **contributions** of each tree
  - Low $\to$ require **more trees** in the *ensemble* to *fit the training set*
    - But **predictions** often *generalize better*.
      - This is a **regularisation** technique called **==shrinkage==**. 
- Figure below shows two GBRT ensembles trained with a **low learning rate**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211202190855735.png)

- Left $\to$ does **not have enough trees**
- Right $\to$ has **too many trees**
- To find the **optimal number of trees**
  - You can use *early stopping* (stopping at the minimum validation in training to prevent overfitting)
- Simple method to **implement** is the `staged_predict()` method
  - Returns an **iterator** over the **predictions** made by the **ensemble** at *each stage* of **training** (*one tree, two trees*)
- Following code $\to$ trains a **GBRT** ensemble with **120 trees**
  - The measures the **validation error** at **each stage** of *training* to find the **optimal number of trees**
    - Finally $\to$ **trains** another **GBRT** ensemble using the **optimal number of trees**.



```python
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_squared_error

X_train, X_val, y_train, y_val = train_test_split(X, y)
gbrt = GradientBoostingRegressor(max_depth=2, n_estimators=120)
gbrt.fit(X_train, y_train)

errors = [mean_squared_error(y_val, y_pred)
          
for y_pred in gbrt.staged_predict(X_val)]
    bst_n_estimators = np.argmin(errors)
    gbrt_best = GradientBoostingRegressor(max_depth=2,n_estimators=bst_n_estimators)
    gbrt_best.fit(X_train, y_train)
```



- The **validation errors** are *shown on the left below*
- The **best model** *predictions* are **represented on the right**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211202191737821.png)

- Possible to **implement** *early stopping* by **stopping training** early
  - Rather than training a large number of trees first then looking back for optimal number
- Setting `warm_start = true` 
  - Makes sklearn **keep** the **existing trees** when the `fit()` method is called
    - Allowing **incremental** training
- Code below **stops training** when the **validation error** does *not improve* for **five iterations in a row**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211202192023656.png)

- The `GradientBoostingRegressor` supports a **subsample** *hyperparameter*
  - This **specifies** the **fraction** of **training instances** to be used for **training each tree**
- Example $\to$ if `subsample = 0.25` 
  - The **each tree** is *trained* on $25\%$ of the **training instances** 
    - These are **selected randomly**.
- This **trades** some *higher bias* for a *lower variance*
  - Also speeds up **training considerably**
    - This is called **==Stochastic gradient boosting==** 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211202192450367.png)

- Worth **noting** $\to$ an **optimized** implementation of **gradient boosting** is there
  - `XGBoost` $\to$ meaning **Extreme gradient boosting** 

```python
import xgboost

xgb_reg = xgboost.XGBRegressor()

xgb_reg.fit(X_train, y_train)

y_pred = xgb_reg.predict(X_val)
```

- Nice features included that take care of **early stopping automatically**

```python
xgb_reg.fit(X_train, y_train,
            
eval_set=[(X_val, y_val)], early_stopping_rounds=2)

y_pred = xgb_reg.predict(X_val)
```

## Stacking

- *stacked generalization* 
  - Based on idea
    - Instead of using **trivial functions** such as *hard voting* $\to$ for **aggregregate predictions**
      - Train a **model** to perform this **aggregate** for us.
- Figure below shows an **ensemble** performing some **regression task** on a **new instance**
  - Each of the **bottom three predictors** 
    - Predict some **different value** ($3.1$ , $2.7$, $2.9$) 
      - Then the **final predictor** called a **==blender==** or **==meta learner==** 
        - Takes the predictions as inputs and makes the **final prediction**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211202193139988.png)

- To **train a blender**
  - Common idea $\to$ use a *hold out set*

- First **training** set split into **two subsets**
  - The **first subset** $\to$ used to **train the predictors** in the **first layer**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211202193238707.png)

- The **first layer** predictors are used to **make predictions** on the **second** *held out subset*
  - This **ensures** that the **predictions** are *clean*
    - Since the **predictors** never saw these instances during training
- For **each instance** $\to$ in the *hold out set*
  - There are **three predicted values**
    - Create a **new training set** using these as **local input features** (makes the new training set **three dimensional**)
      - Alongside keeping the **target values**
- The **blender** is *trained* on the **new training set**
  - Therefore it **learns** to **predict** the **target value** given the **first layers predictions**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211202194304488.png)

- Possible to **train several blenders** like this
  - Such as using **linear regression**, with another using **random forest regression** etc.
- Obtain a **whole layer of blenders**
  - The *idea* is to **split** the **training set** to **three subsets**
    - First $\to$ used to **train** the **first layer**
    - Second $\to$ used to **create** the **training** set used to **train** the **second layer** (*using predictions* that are *made by the predictors of the first layer*)
    - Third $\to$ using predictions made by the predictors of the second layer
  - Once complete
    - Can make a prediction for a **new instance** by **going** through *each layer sequentially*
      - As *shown* in figure below.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211202194641609.png)

- Sklearn does **not support stacking directly**
  - Not hard to roll out this **implementation**
    - Can use an **OS** implementation called `brew` 

