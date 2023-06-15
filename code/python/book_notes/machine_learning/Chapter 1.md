# Fundamentals of machine learning

## Why use machine learning

- Traditional approach

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104200148897.png)

- Automatically learn from **words** and **phrases** which are used as **features** to **predict**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104200400573.png)

- Proper method to use for this.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104200418572.png)

- Built to adapt to change.

---

- Used for **complex** problems impossible for **traditional approach** such as **speech recognition**. 
  - Look for certain **pitches** in sounds to **extract** what *words / letters* they have said are.
    - Cant hardcode algorithms for all situations therefore make a system **that can learn**.

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104200721616.png)

- Help humans learn.
  - Such as in **data mining**.

---

- Summary, ML great for:
  - Problems for which **existing solutions require a lot of hand-tuning** or long lists of
    rules: one Machine Learning algorithm can **often simplify code** and perform better.
  - **Complex problems** for which there is **no good solution** at all using a **traditional**
    **approach**: the best **Machine Learning techniques** can find a solution.
  -  **Fluctuating environments**: a Machine Learning system can **adapt to new data**.
  - Getting **insights** about **complex** problems and **large amounts of data**.

## Types of machine learning systems

### Supervised Learning

- The **training data** you feed to **algorithm** includes **desired solutions** called *labels*.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104201055385.png)

- Example is **classification** for a **spam filter**.
- Example is also **target numeric value** such as **car price** / **median house income**.
  - Given set of *features* called *predictors* 
    - Called **==regression==**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104201222631.png)

#### Example algorithms

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104201249031.png)

- Some **neural networks** are **unsupervised** such as **autoencoders** and **restricted Boltzmann machines** 

### Unsupervised learning

- Data is **unlabelled**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104201345305.png)

#### Example algorithms

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104201407291.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104201613125.png)

- Lots of data on a **blogs visitors**
  - Use a **clustering algorithm** $\to$ detect the **groups** of **similar visitors**
    - Do *not tell which group* they are a part of.
      - Finds **interesting insights** that are **potentially unexpected**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104202114628.png)

- **Visualisation** *algorithms* are good examples of **unsupervised learning**
  - Feed lots of **complex** and **unlabelled data**
    - Output 2D/3D representation of the data whilst **trying to preserve structure**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104202234172.png)

- Similar task is **dimensionality reduction**
  - To *simplify data* with **minimal information loss**
    - Idea is **merging** of **several correlated** *features* into **one**. 
      - Sees what features **correlate** lots with others and **merge** $\to$ called **==feature extraction==** 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104203224348.png)

- *Anomaly detection* is also **unsupervised learning** such as *fraud prevention* / *manufacturing defects*
  - Removes **outliers** automatically.
    - Learns **normal instances** and learns to **recognize** the **out of ordinary** instances.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104203410109.png)

- Many other examples to explore.

### Semi-Supervised Learning

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104203917718.png)

- Combinations of the **two mentioned**
  - Example is *deep belief networks* based on **unsupervised components** called *restricted boltzmann machines* stacked on one another.
  - RBM trained **sequentially** in **unsupervised manner** then the system is **tuned** via **supervised learning** *techniques* 

### Reinforcement learning

- Uses an **agent** that **observes** the **environment** 
  - Selects / performs **actions** and obtains **rewards** for them, alternatively **penalties** for them too,.
- Uses a **policy** to obtain the **most reward over time**.
  - Policy defines **actions** given some **state** where **state** is constructed from the **algorithm** they use.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104204416086.png)

- ​	Often used in **robots** 
  - DeepMind AlphaGo uses **reinforcement learning**
    - Learned the **winning policy** based on **analysis** of **many games**.

### Batch / Online Learning

- Whether they **incrementally** learn from **incoming stream of data**

#### Batch learning

- Can **not learn incrementally**

  - Trained on **all available data**
    - Takes a **lot of time often** therefore performed **==offline==**
- Train $\to$ launch $\to$ with **no further learning** just **applying** its model its *offline learning*.

  - Must be **retrained** again on the **entire dataset**.
- This process can **be automated** easily so can **adapt to learn** 
  - based on **fix learning schedule**.
- Requires **various resource considerations**.

#### Online learning

- Train systems **incrementally** by **feeding data** instances *sequentially*
  - May be **individual** or *small groups* of *mini batches*
    - Each **learning step** is *fast / cheap* and **can learn** the data **fast** as it **arrives**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104205323846.png)

- Ideal for **stock prices** that receive a **continuous flow** of data.
- Must **adapt to change** within *rapid nature*.
- Good in **limited resource computers**.
- Once it has **learned something** it *does not need retraining* $\to$ saves **lots of spaces**
- Trains systems that **cant fit** in **main memory** called **==out of core learning==**. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104205737879.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104205648483.png)

- Parameter to consider is the how fast they are learning
  - How fast they **adapt** to **new data** is the **==learning rate==**.
- Online learning issue is **bad data** where it **cripples the model** over time.
  - May be from a **malfunctioning sensor** on a **robot possibly**.
  - Spamming a **search engine** to attempt to **reach high results**.

- Must **monitor** closely and **switch learning off** if this occurs 
  - Noticed via **performance drop**
- Can **monitor input data** via *anomaly detection algorithm.*

### Instanced based versus model based learning

- How well models **generalize** is **important** 

#### Instanced based learning

- Rather than **learn by heart** $\to$ measure **similarities** of **training data**.
- Base the **classification** on **having a certain number of features**
- Called **==instance based learning==**
  - Learns examples **by heart** then **generalizes** the *new ones* by comparing the **similarities** of to learnt ones.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104210426938.png)

### Model based learning

- Build a **model** of the **examples** to be used to make **predictions** called **==model based learning==**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104210547850.png)

- Example plot satisfaction over money earned from data set

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104210831070.png)

- There is trend, although **noisy**
  - Relatively **linearly increases**. 
    - Solve via **linear regression** or **polynomial potentially**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104211031276.png)

- Variety of **linear models** to solve this issue.
  - Defined on **features** that influence the **parameters** 
- Requires a **cost function** to determine **how wrong it was**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104211247921.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104211302047.png)

- Are the **optimal parameters found to fit**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104211221773.png)

- After finding optimal values of our **linear model**.

---

- Model for learning how happy Cypriots are for whatever the fuck reason. 

```python
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
import sklearn.linear_model

# Load the data
oecd_bli = pd.read_csv("oecd_bli_2015.csv", thousands=',')
gdp_per_capita = pd.read_csv("gdp_per_capita.csv",thousands=',',delimiter='\t',
encoding='latin1', na_values="n/a")

# Prepare the data
country_stats = prepare_country_stats(oecd_bli, gdp_per_capita)
X = np.c_[country_stats["GDP per capita"]]
y = np.c_[country_stats["Life satisfaction"]]

# Visualize the data
country_stats.plot(kind='scatter', x="GDP per capita", y='Life satisfaction')
plt.show()

# Select a linear model
model = sklearn.linear_model.LinearRegression()

# Train the model
model.fit(X, y)

# Make a prediction for Cyprus
X_new = [[22587]] # Cyprus' GDP per capita
print(model.predict(X_new)) # outputs [[ 5.96242338]]
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104211834626.png)

- Summary of the project

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104211727292.png)

## Main challenges of ML

- Bad algorithm / Bad data

### Insufficient quantity of training data

- need lots to be relevant basically.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104212202785.png)

### Non representative training data

- Must have data that represents the **new cases** also.
  - True for both **model** and **instance based** *learning*. 
- Countries used **earlier** for **training** the **linear model** was *not perfectly representative*
  - A **few countries** were **missing**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104212432632.png)

- This dotted line is **unrepresentative**
  - Shows that **missing data** can *misinform* you on insights.
    - Regarding the **lower and upper ends** of the scale.

---

- Crucial for some **training set** to be **representative** of *cases* you wish to **generalize**
  - Often **harder** than sounds
    - Too small a sample $\to$ large **sampling noise** $\to$ *non representative* data from **chance**
    - Large can also be not representative from **bad sampling** from **==sampling bias==** 

### Poor quality data

- Many errors / outliers / noise from **bad measurements**
  - Much **harder** for a **system to learn** and detect **underlying patterns**. 

### Irrelevant features

- Must perform **feature engineering** and **cleaning** (*poor quality data*)
  - feature selection $\to$ select most useful features to train on among existing features
  - feature extraction $\to$ combine existing features to **produce more useful ones**
    - Dimensionality reduction used here for example.

### Overfitting data

- Complex models such as **neural networks** can **detect small changes** but may be *noise* and **not generalize well**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104213357389.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104213408493.png)

- Constraining model to not overfit is **regularization**.
- Reduce number of parameters in our example above such as **2 > 1** , must fit based on that which is **harder**
- But **potentially more general** and **therefore** performs better on **new cases**. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104213951862.png)

- The **degree** of *regularization* to use is based on a **hyperparameter**.
  - The **parameter** to the actual **learning algorithm** we are **utilising**.
- Setting high fucks your model though.

### Underfitting 

- Opposite of overfitting

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104214128867.png)

## Testing and Validating

- Split data to **two sets** for **training  / test**
- **Error rate** on *new cases* is the **generalization error** (*out of sample error*)
- Evaluating model on **the test set** $\to$ obtain **estimate** of **this error**
- Tells you **how well** the **model performed** on *instances* it has **never seen before**.
- If **low training error** as in it makes **few mistakes** but **generalization is high** therefore its **overfitting**

### Hyperparameter Tuning and Model selection.

- Deciding a model to use
  - Train both and **compare** how well they **generalise** using the **test set**.

- Say **linear model generalizes better**
  - Wish to apply **some regularization** to *avoid overfitting*
    - How would you **choose** the **hyperparameter** 
      - Train on 100 models that are different with 100 different values
- Find best hyperparameter value that **produces** a model with **lowest generalization** at just **5% error**
  - But it produces **15%** errors in **production**.
    - Our issue is **we measured generalisation** error *multiple times* on the **test set**
      - The adapted **the model** and **hyperparameters** to produce the **best model** for our **particular set**
        - Therefore **unlikely** to perform that well **on new data**

---

- Solution is **holdout validation** where you hold **part** of the **training set** to *evaluate candidate models* to *select* the **best one**

  - called the **==validation set==**

- Train *many models* with *various hyperparameters* on **reduced training set** (*train set* without the validation set)

  - Select model that **performs** the best on the **validation set**

- Select after *holdout validation* $\to$ train **best model** on the **full training set** including **validation set**
  - Gives us the **final model**
- Finally just **evaluate** the final model on **test set** to obtain an estimate of the **generalisation error**

---

- If validation set too small $\to$ model evaluations are **imprecise** 
  - May select the **suboptimal model** on mistake
    - Conversely $\to$ if the **validation set** is **too large** the the **remaining training set** is *much smaller* than **full training set** this means the data is **already trained on ** and therefore is **not new data**

- Perform **repeated** *cross validation instead* using **little validation sets**
  - Every model is **evaluated** *once* per **validation set**
    - After being **trained** on the **rest of the data**.
      - Then just **average** all the model evaluations $\to$ obtain a **model** with a **much more accurate measure** of **performance** 
        - Drawback $\to$ **training time** is **multiplied** upwards for **every validation set**. 

---

### Data mismatch

- Using online data of general data such as flowers is easy
- Say you get 10,000 flowers , hard to **be representative** for every possible photo taken in the app.
- Remember that the **validation set** and **test set** must **be representative** of the data in production.
- Composed **exclusively** of **representative pictures** 
  - Shuffle and place **half** in **validation** set and **half** in the **test set**.
  - Making sure that **no duplicates** or **near duplicates** end up in **both sets**.
- After **training your model** on the web pictures, if you **observe that the performance**
  of your **model** on the **validation** set is **disappointing**, you will not know
  whether this is because your **model has overfit the training set**, or whether this is just
  due to the **mismatch between** the web pictures and the mobile app pictures.
- One solution is to **hold out part of the training pictures** (from the web) in yet another set that
  Andrew Ng calls the **train-dev set.** 
- After the model is **trained** (on the training set, noton the train-dev set), you can **evaluate it on the train-dev set**: if it **performs well**, then the model is **not overfitting** the **training set**, so if **performs poorly** on the **validation**
  **set**, the problem must come from the **data mismatch.** 
- You can try to tackle this problem
  by **preprocessing** the **web images** to make them **look more like the pictures** that
  will be taken by the **mobile app, and then retraining the model.**
-  Conversely, if the
  model performs **poorly** on the **train-dev set**, then the model must have **overfit** the
  **training set**, so you should try to **simplify or regularize the model,** get **more training**
  **data and clean up the training data, as discussed earlier**

