# Classification

## Mnist

- Set of **70,000** small images of digits that are **handwritten** 
- Often the **hello world** of **machine learning**
- Many algorithms are **tested** based on **this set**.

---

- Can fetch using **sci-kit** to fetch this data

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104224117137.png)

- Datasets loaded by **scikit-learn** generally have **a similar** *dictionary* structured including:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104224310789.png)

- View these arrays:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104224404802.png)

- There are **70,000** images  with **784 features** as the picture is $28 \times 28$ pixels
- Each measures the **intensity** from $0$ to $255$ (*white* $\to$ *black*) 
- View some **single digit** just grab an **instances** *feature vector* then `reshape` it to $28 \times 28$ and **display** using `imshow()` 

```python
import matplotlib as mpl
import matplotlib.pyplot as plt

some_digit = X[0]
some_digit_image = some_digit.reshape(28, 28)

plt.imshow(some_digit_image, cmap = mpl.cm.binary, interpolation="nearest")

plt.axis("off")
plt.show()
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104224812959.png)

```python
>>> y[0] # extract the label used here.
'5'
```

- Prefer to use **numbers** rather than a **string**

```python
>>> y = y.astype(np.uint8)
```

- Form the **train / test set**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104225258300.png)

- Training set is **shuffled already** as it **ensures** that every **cross validation folds** *will* familiar
  - You *dont want one fold* to **be missing some digits**
- Learning algorithms are **sensitive** to the **order** of **training** *instance* 
  - They perform poorly if they obtain **many similar instances** in a *row*
    - *Shuffling* **the dataset** ensures **this wont happen**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104231950430.png)

## Training a Binary Classifier

- Simplifying the **problem** and *identify* a **single digit**
  - For example $\to$ number $5$ 
    - An example of **==binary classifier==** $\to$ capable of **distinguishing** between *two classes* $5$ and $\neg 5$ 
- Create **the** *target vectors* for **classification task**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104230233667.png)

- Select some **classifier** and train it
  - Example $\to$ **stochastic gradient descent** 
    - Handles **large values effectively**.
    - Deals with training **instances** *independently*  , one at a time.
- Create `SDGClassifier` 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104230533324.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104230703013.png)

- Now use **to detect** the images of the **number** $5$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104230904255.png)

- Classifier guess the image represents a **5** 
  - Guessed **right in this particular case** $\to$ evaluate models performances.

## Performance measures

- Evaluating a **classifier** is **significantly trickier** than *evaluating a regressor*
  - spend a **large part** of **this chapter** on this topic
    - There are **many performance** measures available.

### Measuring accuracy using cross validation

#### Implementation

- More **control** required for **cross validation process** than what **scikit learn** provides **off the shelf**
- Implement **cross validation** yourself
- Quite **simple**, following code does **roughly same** thing as `cross_val_score()` from scikit learn.

```python
from sklearn.model_selection import StratifiedKFold
from sklearn.base import clone

skfolds = StratifiedKFold(n_splits=3, random_state=42)

for train_index, test_index in skfolds.split(X_train, y_train_5):
    # clone model
    clone_clf = clone(sgd_clf)
    
    # select the test and train set based on the folds
    X_train_folds = X_train[train_index]
    y_train_folds = y_train_5[train_index]
    X_test_fold = X_train[test_index]
    y_test_fold = y_train_5[test_index]
    
    # fit the model based on this fold
    clone_clf.fit(X_train_folds, y_train_folds)
    
    # calculate the prediction
    y_pred = clone_clf.predict(X_test_fold)
    
    # sum the correct values
    n_correct = sum(y_pred == y_test_fold)
    
    # print average correct over number of samples.
    print(n_correct / len(y_pred)) # prints 0.9502, 0.96565 and 0.96495
```

- `StratifiedKFold` performs **stratified sampling**
  - Produces **folds** that **contain** a **representative** *ratio* of **each class**
    - For *every iteration* the **code** creates a **clone** of the *classifier* 
      - **Trains** that clone on the **training folds** $\to$ *makes predictions* on the **test fold**
        - **Counts** the **number** of *correct predictions* and outputs the **ratio** of **correct predictions**.

---

- We now use `cross_val_score` to evaluate your `SDGClassifier`  via **K-fold** **cross-validation** using **3 folds**
  - *K-fold* **cross validation** involves **splitting** the **training set** to *K-folds* (for this case **its 3**)
    - Then **making predictions** and **evaluating them** on *each fold* using a **model trained** on the **remaining folds**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211104235753105.png)

- Above **93%** accuracy (*ratio* of **correct predictions**) 
  - on all **cross validation folds**

---

- Look at simple classifier that just **classifiers** each image in the **not 5** class

```python
from sklearn.base import BaseEstimator

class Never5Classifier(BaseEstimator):
	def fit(self, X, y=None):
		pass
	def predict(self, X):
		return np.zeros((len(X), 1), dtype=bool)
```

- Model accuracy:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211105000942271.png)

- Has over **90% accuracy** as only about **10%** of the images are **5**.
- This demonstrates why **accuracy** is often  *not* the **preferred** performance measures of **classifiers**
  - Especially for **==skewed datasets==** $\to$ some classes appear **much more often** than others.

### Confusion matrix

- Evaluate **performance** via *confusion matrix*
  - Idea $\to$ **count** the **number of times** *instances* of class $A$ are **classified** as $B$ 
    - To find the **times** that the classifier confused **5** with **3**.
    - Look in the $5^{th}$ row and $3^{rd}$ column of **confusion matrix**.

---

- To compute it
  - Obtain a **set** of *predictions* to be **compared** to **actual targets**
  - Could be on the **test set** *itself* but **leave alone for now**. (only use at the end)
- Use `cross_val_predict` 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211105002045366.png)

- Performs **K-fold** *cross validation*

  - Rather than returning **the evaluation scores**
    - Returns the **predictions** made on *each test fold*.

- Obtain a **clean prediction** for *each instance* within the **training set**

  >  *Clean* = prediction via a **model** that *never saw the data* during **training**. 

- Obtain **confusion matrix** via `confusion_matrix` function.
- Passing it the **target classes** and **predictions**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211105002411566.png)

- Every *row* is an **actual class**.

- Every *column* is a **predicted class**.

  - `[0][0]`= $\neg 5$ images (**negative classes**) = $53,057$ were classified as $\neg 5$  = **==true negatives==** 
  - `[0][1]` = correctly **classified** at $1522$ are **==false positives==** 

  - `[1][0]` = $5$ images (**positives**) = $1235$ classified **correctly** are **==false negatives==**  
  - `[1][1]` = correctly classified as being 5 therefore are **==true positives==** 

- A **perfect classifiers** has only **true positives** and **true negatives** 
  - Therefore has **only values** that are $\neq 0$ on the **diagonal**. from to left.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211105003620790.png)

- **Confusion matrix** gives *lots of information*
  - May prefer a **more concise metric**
    - Interesting one is **the accuracy** of the **positive predictions** called the **==precision==** of the **classifier**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211105003754015.png)

- $TP$ is the **number** of **true positives**, $FP$ = **false positive count**. 

---

- Best way for **perfect precision** is to obtain a **single positive** *prediction* and **ensures** its **correct**
  - Useful as the **classifier** would *ignore* every one but **one positive instance**
    - Thus **precision** is **typically** used along with **==recall==** aka *sensitivity* or *true positive rate*. $TPR$ 
      - The **ratio** of *positive instances* that are **correctly** *detected* by the *classifiers*. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211105004524815.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211105004614362.png)

### Precision and Recall

- Several functions to **computer classifier metrics**
  - Includes **precision** and **recall**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211105005037208.png)

- Now the **5 detector** is **not as good** when viewing its **accuracy**
  - Claims an **image** *represents* $5$ its only correct **72.9%** of the time
  - Also only **detects** **75.6%** of the $5$'s

---

- Convenient to **combine** *precision / recall* to some **single metric** called **==$F_1$==** **==score==**.
  - A **simple method** of *comparing two classifiers*

- The $F_1$ score is a *harmonic mean* of *precision / recall*
  - The **regular mean** treats each **value equally**, harmonic gives **more weight** to *low values*
    - Therefore only obtains a **high** $F_1$ score if **both** *metrics* are **high**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211105005554912.png)

- To computer, call `f1_score()` function

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211105005714028.png)

- Favours **classifiers** that have **similar precision / recall**
  - Not always favourable.
- May wish for **low recall** for *safe videos* with **high precision** instead
- **high recall** may let some *bad videos* pass 
- **shoplifting surveillance** detection requires **higher recall** than **precision** as its *unlikely* to ensure **all are caught**
  - May be **false alerts** however of course.

---

- You *cannot have it both ways*
  - increasing *precision* **reduces recall**, vice versa = **==precision/recall tradeoff==** 

### Precision/Recall Trade-off

- Understand via analysing `SDGClassifier` making its **classification decisions**
  - Each instance $\to$ computers a *score* based on the *decision function* 
    - If its **greater** than some **threshold** $\to$ assigns said **instance** to the *positive class*, else *negative*.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211111004453038.png)

- Shows some **digits** *positioned* from **the lowest** score *left* to **highest** on *right*
- Say this *decision threshold* is actually on the **arrow** in the *center* between the two 5's.
  - You find **4 TP** (*actual 5's*) on *right* and **one false positive** on this side which is a **6**.
  - Therefore with **this threshold**, the *precision* is just **80%** 
  - But out of 6 actuals 5's the **classifier** only detects **4** therefore *recall* is **67%** 
  - If you **raise the threshold** $\to$ the *false positive* becomes a **true negative** making **100% precision**
  - At the **expense** of *recall* as a **false negative** crops up in the **5**.

- increase precision = reduced recall thus.

---

- *Scikit* does *not enable* us to **set thresholds** *directly*  
  - Gives us *decision scores* that it **uses** in **predictions**
- Rather than calling `predict()` $\to$ use `decision_function()` 
  -  Returns a **score** for *each instance*
    - Then makes **predictions** based on those scores using **thresholds** that *we define*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211111005223791.png)

- The `SDGClassifier` uses a **threshold** equivalent to $0$ 
  - Therefore **previous code** returns *same result* as with `predict()` method
    - Lets **raise the threshold** 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211111005816099.png)

- This image **represents** a **5** but **precision ^** did **not account for it**

---

- How to *decide* on a **threshold**
  - Obtain the **score** of *all instances* in the **training set** via `cross_val_predict()` function
    - his time $\to$ just **specify** you want it to **return** some *decision scores* rather than **predictions**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211111010035088.png)

- From these **scores**
  - compute the *precision / recall* for **all possible thresholds** via `precision_recall_curve()` function 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211111010123558.png)

- Then **plot** the **precision** and **recall** as **functions** of the **threshold** value using **matplotlib**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113003420609.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113003429974.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113003456703.png)

- So precision just counts the fraction of correct guesses (TP) over the number of guess over the threshold (TP+FN)
  - Thus If increasing threshold removes a TP value then it ill **decrease precision** in **most cases**.

---

- Another method to **select** a **good / precision recall** *tradeoff* is plotting the **precision** *directly* **against** the **recall**
  - The *same threshold* is *highlighted*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113004203564.png)

- Precision *starts* to **fall sharply** around the $80\%$ *recall*
  -  Select a **precision / recall** *tradeoff* just **before** this *drop* at around $60\%$ recall.
    - *depends* on the **project** in **question**.

---

- Say you attempt to **aim** for $90\%$ *precision*
  - Look up the **first plot** and find a **threshold requirement** of around $8000$ 
    - Search for the **lowest threshold** that gives you **at least** $90\%$ *precision* 
- `np.argmax()` gives us the **first index** of the **maximum value** , in this case mean the **first true value**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113004633354.png)

- **To make predictions** (*training set*)
  - Rather than **calling** the *classifiers* `predict()` 
    - Just **run this code**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113004727550.png)

- *Check* these predictions **precision / recall**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113005104889.png)

- A **90%** *precise* **classifier** 
  - Fairly simple to **adjust** the **precision** to how you **want**
    - A **high precision classifier** is *useless* with **low recall** though

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113005242895.png)

### The ROC curve

- The *receiver opearting characteristic* ROC
  - A **common tool** used for **binary classifiers**
    - Similar to the **precision / recall curve**
    - Rather than plotting **precision vs recall** it plots the **==true positive rate==** (*recall*) against **==false positive rate==**

- The **FPR** is the *ratio* of **negative instances** that are *incorrectly classified* as **positive**
  - It is $1- \text{true negative rate}$ where the **TNR** is the **ratio** of **negative instances** that are **correctly classified** as *negative*
    -  The **TNR** is also a *specificity* 
      -  Hence the **ROC** curve plots **sensitivity** (*recall*) versus $1-\text{specificity}$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113005901761.png)

---

- To **plot** the **ROC** curve
  - First compute the **TPR** and **FPR** for *various thresholds* via `roc_curve()` 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113010020003.png)

- Then **plot** the **FPR** against the **TPR** 
  - produces **figure 3.6**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113010049881.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113010100364.png)

- There is of course some *tradeoff*
  - The **higher** the **recall** $(TPR)$  $\to$ the **more** **false positives** $(FPR)$  the *classifier produces* 
    - The **dotted line** represents the **ROC** curve of a **purely random classifier**
      - A *good classifier* stays **far away** from this line as **possible** (*top left corner*).

---

- One method to **compare classifiers** is measuring the **area under the curve** $(AUC)$ 
  - A **perfect classifier** has a **ROC AUC = 1** 
  - A **random classifier** has a **ROC AUC = 0.5** 
- scikit-learn has a **function** to **compute** the **ROC AUC**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113011127613.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113011355841.png)

- Training a `RandomForestClassifer` and compare **ROC curve** and **ROC AUC** score to the `SGDClassifier` 
  - Obtain **scores** for **each instance** in *training set*
  - `RandomForestClassifer` does **not have** some `decision_function()` 
    - Has `predict_proba()` returning an **array** containing a **row / instance** and **column / class** 
      - Each containing the the **probability** that a **given instance** *belongs* to some **given class** (*70%* chance that an image represents a $5$) 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113011849560.png)

- To **plot** some **ROC curve** $\to$ requires **scores** ,**not probabilities** 
  - A *solution* is to use the **positive class's** *probability* as the **score**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113012245967.png)

- Ready to **plot** the **ROC curve**
  - Useful to plot the **first ROC curve** as well to **see how they compare.**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113012522170.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113012603808.png)

- **dotted line** = **random classifier**.

---

- The `RandomForestClassifer` **ROC curve** looks *much better* than the `SDGClassifier` 
  - Comes **much closer** to the **top left corner**
    - As a *result* $\to$ its **ROC AUC** *score* is **significantly better**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113013002700.png)

- Measuring the **precision / recall** of this will find $99\% \ precision$ and $86.6\% \ recall$

---

#### Summary of Binary training

- Choose **approriate metric**
- Evaluate the **classifiers** using **cross validation**
- select the **precision  / recall** to **fit the needs**
- Compare the **various models** using **ROC curves** and **ROC AUC scores**

## Multiclass Classification

- Algorithms such as **Random forest** and **naïve bayes** can *directly handle many classes*
- Some such as **support vector machines** or **linear classifiers** are strictly *binary classification*

---

- One method to **support multiple classes**
  - Example $\to$ for **10 classes** is training **10 binary classifiers** (0 detector, 1 detector)
    - Then when **you classify**, obtain a **decision score** from *each classifier* and **select highest one**.
      - Called **==one versus all==** $(\text{OvA})$ or **one versus the rest**.
- Another method is to **train a binary classifier** for *every pair of digits* 
  - One to **distinguish** $0$ and $1$ / $0$ and $2$ 
    - Another for $1's$ and $2's$ 
      - Called **==one-versus-one==** $(\text{OvO})$ *strategy*.
  - If there are $N$ classes you must **train** $N \times (N-1) / 2$ *classifiers* 
    - For **MNIST** this trains **45 binary classifiers** and *see* which **class wins** the *most duels* 
      - The *clear advantage* of **OvO** is that **each classifier** must only be **trained** on the **part** of the **training set** for **two classes** it *must distinguish*.

---

- Some **algorithms** (such as **SVM**) $\to$ *scale poorly* with the **size of the training set**
  - Therefore for **these algorithms** $\text{OvO}$ is *preferable* as its **faster** to train **many classifiers** on *small training sets*
    - Than *few classifiers* on **large training sets**.
- For **most binary classification algorithms**
  - $\text{OvA}$ is **preferred** 

---

- Scikit-learn detects when you **try to use a binary classification algorithm** for **multiclass classification task**
  - It **automatically** runs $\text{OvA}$ (apart from **svm** which is does $\text{OvO}$ ) 
    - Attempt with `SDGClassifier`

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113015052204.png)

- This code **trains** the `SGDClassifier` on the **training set** using the **original target classes** from $0$ to $9$ (`y_train`) 
  - Instead of the **5 versus all** target classes (`y_train_5`) 
    - Then makes a **prediction** (*correct one in this case*)
- Under the hood $\to$ scikit-learn actually **trained** $10$ *binary classifiers* 
  - Obtained the **best decision scores** for the **image** then selected the **one with the largest score**.

---

- To **see why this is the case**
  - Call `decision_function()` method
    - Rather than **returning** just **one score per instance** $\to$ returns **10 scores** for **each class**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113015739771.png)

- The **highest score** is that **one corresponding** to $5$ clearly.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113015805443.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113015825381.png)

- To **force** *scikit learn* to use **one versus one** or **one versus all**
  - Can use the `OneVsOneClassifier` or `OneVsRestClassifier` *classes*.
    - Just **create an instance** and **pass a binary classifiers** to the **constructor**
- Example $\to$ code creates a **multiple class** *classifier* using $\text{OvO}$ strategy based on a `SGDClassifier`   

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113020239482.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113020248753.png)

- Training a `RandomForestClassifier` is **just as easy**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113020322848.png)

- This time scikit-learn did **not run** $\text{OvA}$ or $\text{OvO}$ because **random forest classifiers** *can directly classify* instances into **multiple classes**
  - Can **call** `predict_proba()` to obtain the **list of probabilities** that the *classifier* assigned to **each instance** for **each class**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113020702842.png)

- See that the **classifier** is *fairly confident* on its **prediction**
  - Values are **percentages** on **it being that class**.

---

- Must also **evaluate** *these classifiers*
  -  As *usual* $\to$ using **cross validation**
    - Evaluate `SDGClassifiers` accuracy using`cross_val_score()` function

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113020855547.png)

- Obtains over $84\%$ on **all test folds**
  - If you used a **random classifier** you would obtain $10\%$ accuracy
    - Therefore its **not such a bad score**, but can **do still do better**
      - Example $\to$ to *scale the inputs* $\to$ **increases** *accuracy* above $89\%$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113021209267.png)

## Error Analysis

- Proper ML project checklist you would `GridSearchCV` and **automating** as *much as possible*
- We assume here you **have found some promising model** and you want **ways** to **improve it**
- One **way** *to do this* is to **analyze** the **types** of **errors** it *makes*.

---

- First analyse the **confusion matrix**
  - Must *make predictions* using the `cross_val_predict()` 
  - Then call the `confusion_matrix()` just like with **earlier**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113140920035.png)

- There are **many numbers**
  - Often *more convenient* to **look at an image representation** of the *confusion matrix* 
    - Use `matshow()` function.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113141343345.png)

- Confusion **matrix** *looks fairly* good as **most images** are on the **main diagonal**
  - This means they are **classifier correctly** 
    - The $5's$ look **slightly darker** which may **mean** that there **are fewer images** of $5's$ in the **dataset** or that *classifier* does **not perform** as **well** on $5's%$ as on the **digits**
      - Verify that **both case.**

---

- Focus the **plot on the errors**
  - Must **divide each value** in the **confusion matrix** by the *number of images* in the **corresponding class**
    - Therefore you **compare error rates** rather than the **absolute number of errors** (*which makes abundant classes look unfairly bad*)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113142300197.png)

- Now **fill the diagonal with zeros** to *keep only the errors*
  - Plot the result.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113142327782.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113142453273.png)

- Clearly see the **kinds of errors** the *classifier makes*
  - Remember the **rows** represent the **actual class** while **columns** represent the **predicted classes**
- The **columns** for class $8$ is **quite bright** 
  - Tells you **many images** are **misclassified** as $8's$ 
    - How ever the **row** for **class 8** is **not bad**.
      - Telling you **that actual 8s** in *general get properly* classified as 8s
- As **you can see** the **confusion matrix** is *not necessarily*  **symmetrical**
  - You can see that $3's$ and $5's$ often **get confused** (*in both directions*)

---

- **Analysing** the **confusion matrix** can often *give insights* on methods to **improve** your **classifier**.
  - Maybe spend time **reducing** the *false 8s* 
    - Example $\to$ attempt to **gather more** training digit that look **likes 8** (*but are not*)
      - This helps it **classify** the **harder images of 8s**
  - Could **engineer** *new features* would **help the classifier** for *example* writing **an algorithm** , to count the **number** of **closed loops** (8 has two, 6 has one, 5 has none) 
  - Or **pre-process** the **images** (such as via *scikit image , pillow or OpenCV*) to make **some patterns stand out more** such as **closed loops.**

- Analysing **individual errors** can be **good** way to **gain insights** on what **your classifier** is *doing* and **why it is falling**
  - However its **more difficult** and **time consuming**
    - For **example** , lets plot **examples** of $3's$ and $5's$ (the `plot_digits()` function that uses **matplotlib** `imshow()`) 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113144549952.png)

- The **two** $5 \times 5$ blocks on the **left show digits** *classified* as $3's$ and the **two** $5 \times 5$ *blocks* on the **right show images** *classified as 5's*
  - Some of the **digits** that the **classifier gets wrong** (such as in the **bottom left and top right**) are so **badly written** even a *human* would **struggle** with the **classification**.

- Most **misclassified** images seem **like obvious errors** to us
  - And its **hard to understand** why the **classifier** makes the **mistakes** *it did* 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113145233589.png)

- Reason is that we used a **simple** *SGDClassifier* which is a **linear model** 
  - All it does is **assign** some **weight per class** to *each pixel* and it **sees a new image** 
    - It just **sums** up the **weighted pixel intensities** to *get a score* for **each class**
      - So since **3's** and **5's** differ **only by a few pixels** $\to$ this **model will easily confuse them**.

- The **main difference** between $3's$ and $5's$ is the **position** of the **small line** that *joins* the **top line** to the **bottom arc** 
  - If you draw a $3$ with the **junction slightly shifted** to the **left** 
    - Classifier **might** *classify* it as a $5$ vice versa.
      - In other words $\to$ the **classifier** is *quite sensitive* to **image shifting / rotation**
        - So **one reduce** the $3/5$ *confusion* would be the **preprocess** the **images** to *ensure* that **they are well centered** and **not too rotated**
          - Probably **help reduce** other **errors as well**.

## Multilabel Classification

- For now, **each instance** has been **assigned** to *just one class* 
  - In some cases $\to$ may wish the **classifier** to **output multiple classes** for **each instance**
    - For example $\to$ consider a **race recognition**
    - What is the **outcome** when it **recognizes multiple faces** in the **same picture**
    - Of course **should** *attach one* **tag per person** it *recognizes*
    -  Say the **classifier** $\to$ has been **trained** to **recognize faces**. 
      - A, B, C
        - Show picture of A, C $\to$ should output $[1,0,1]$ 
          - This is a **multilabel classification system**
- Looking at a **simpler example** here in this case.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113152417233.png)

- This **code** creates a `y_multilabel` array containing **two target labels** for *each digit image*
  - The **first indicates** whether or not the **digit is large** $(7,8,9)$ 
  - The **second indicates** whether or **not** *its odd* 
- Next lines create a `KNeighborsClassifier` instance (*supports* **multilabel classification** but *not all classifiers do*)
  - Train using **multiple target arrays**.
    - Now **make a prediction** and *notice* that it **outputs two labels**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113153025974.png)

- The digit 5 is not large but is odd
- Many methods of **evaluating** a **classifier** such as this
  - Selecting the **right metric** depends on the **project**
    - One method is to **measure** the $F_1$ *score* for **each label** (or any other metric)
    - Then compute the **average score**

- This code computes the average $F_1$ score **across all labels**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113153615654.png)

- Assumes that **all labels** are **equally important**
  - Which *may or not be not the case* 
    - In **particular** $\to$ if you have **more pictures** of *A* than of B / C
    - may give **more weight** to the **classifiers** scores on pictures of A
    - Simple option is to give **each label a weight** equal to its **support** (*number of instances* with this target label)
    - Achieve this via `average = weighted` option instead before code above.

## Multioutput Classification

- Also called **multioutput classification**
  - It is a **simply generalization** of **multilabel classification** where *each label* can be **multiclass**
    -  It can have **more than two possible values**.

---

- Show this by **building a system** to *remove noise from images* 
  - Take as **input** some **noisy data image** and **hopefully** output a **clean digit image**
    - *Represent* as **an array of pixel intensities** just like the $MNIST$ images
      - Notice that **the classifiers** *output* is **multilabel** (*one label per pixel*)

- Each *label* can have **many values** ranging from $0-255$ 
  - Thus an example of **multioutput** *classification system* 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113150913667.png)

- Start by **creating the training / test sets** by taking the $MNIST$ images and **adding noise**
  - To the **pixel intensities** using `NumPy` `randint()` function
    - Target images will be **the original images** 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113151108450.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113151248527.png)

- Look at **an image** from the *test set* 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113151352268.png)

- Left is **noisy** and on the **right is the clean target image** 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211113154541634.png)

- Looks **close enough to target**.
  - This **concludes** our *tour* of *classification* 
    - Hopefully **now know** how to *select good metrics* for **classification tasks**
      - Pick *the appropriate* *precision / recall tradeoff* **compare classifiers**
        - And *more generally* **build** a **good classification system** for *various tasks*

$$



$$

