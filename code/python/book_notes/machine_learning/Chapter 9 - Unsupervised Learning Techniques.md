# Unsupervised Learning Techniques

-  Most data is **unlabelled**
  - Have some input features $X$ but no labels $y$ 

## Clustering

- Analogy $\to$ find a plant in the wild **not seen before** $\to$ notice some more
  - They are **not identical exactly** but *sufficiently similar* to say they may be the **same species** / **genus**
- Dont need expertise to identify what **objects** are *looking similar*
  - This is **==clustering==** 
    - Task of **identifying** *similar instances* and *assigning* them to **clusters** $\to$ *groups of similar instances*

---

- Each instance $\to$ assigned some **group**
  - This is **unsupervised** 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211206003513184.png)

- Consider image here $\to$ on the **left** is the **iris dataset** (*introduced* in chapter 4)
  - Where each **instances species** (*i.e.* class) is **represented** with a **different marker**.
- This is a *labelled dataset*
  - For which **classification** algorithms studied earlier are **well suited in**.
- On the **right** $\to$ is the **same dataset** $\to$ with **no labels**
  - Therefore **cannot use a classification algorithm**
    - We utilities the **clustering algorithms.** 
      - many of them can **easily detect** the *top left cluster*. 
- Also **quite easy** $\to$ to see with **our own eyes**
  - Its *not so obvious* that the **lower right cluster** is actually **composed** of **two distinct sub clusters**
    - That said $\to$ the **dataset** actually has **two additional features** (*sepal and length width*)
      - That are **not represented here** and *clustering algorithms* can make **good use of all features**.
        - Therefore they in fact **identify** the **three clusters fairly well** $\to$ such as using a **gaussian mixture model** $\to$ only $5$ instances out of $150$ are **assigned** to the **wrong cluster**.

---

### Clustering applications

#### Customer segmentation

- Can **cluster** your **customers** *based* on their *purchases / activity on website / ...* 
- Useful to **understand** who **customers** are and **what they need** $\to$ so you can **adapt your products** and *marketing campaigns* to *each segment*
- Example $\to$ this can be **useful** in *recommender systems* to **suggest content** that *other users* in the **same cluster enjoyed**

#### Data analysis

- When analysing a **new dataset** 
  - Often useful to **first discover** *clusters* of **similar instances** as its often **easier** to *analyze clusters* seperately.

#### Dimensionality Reduction

- Once a **dataset** has **been clustered**
  - Often possible to measure each **instances** *affinity* with each cluster
    - Affinity $\to$ measure of **how well** each **fits** into a **cluster**
- Each **instances** feature vector $x$ can be **replaced** with the **vector** of its **cluster affinities** 
  - If there are $k$ clusters then this **vector** is $k$ dimensional
    - This is **typically** much **lower dimensional** than the **original vector** but it can **preserve enough information** for *further processing*

#### Anomaly detection

- Any instance with a **low affinity** to the **clusters** is an **anomaly**
  - Example $\to$ if you have **clustered** the *users* of your **website based** on their **behaviour**
    - Can **detect users** with **unusual behaviour** such as the **unusual number** of **requests per seconds** etc..
- Anomaly detection is **particularly useful** in *detecting defects* in **manufacturing** or **fraud detection**.

#### Semi supervised learning

- If you only **have a few labels** $\to$ you could **perform clustering** and **propagate** the **labels** to *all the instances* in the **same cluster**
  - This **greatly increases** the **ammount of labels** available for a **subsequent** *supervised learning algorithm*
    - Thus improve its **performance**.

#### Search Engines

- Some **search engines** let you **search** for **images** that are **similar** to a **reference image**
  - To **build such a system** $\to$ you *would first* **apply a clustering algorithm** to all images in the **database**
    - Similar images would **end** up in the **same cluster**
      - Then when a **user** provides a **reference image** 
        - All you need to do is **find this image's cluster** using the **trained clustering** model and then **simply return** all the **images** from **this cluster**.

#### Segment an image

- By **clustering pixels** *according* to the **color**
  - Then **replacing each pixels color** with the **mean color** of its **cluster**
    - Its possible to **reduce** the *number of different colors* in the image **considerably**
- This **technique** is used in **many object detection** and **tracking systems**
  - Makes it easier to **detect contours of each object**.

---

- There is **no single definition** on a cluster
  - Depends **contextually**.

- Some algorithms look for **instances** around some **point** $\to$ a **==centroid==**
- Others look for **continuous regions** of *densely packed instances*
  - These clusters can **take any shape**
- Some are **hierarchical**  looking for **clusters of clusters** etc...

### K-Means

- Unlabelled dataset in figure:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211206011108231.png)

- There are **clearly** $5$ blobs of **instances**
- The $k-means$ algorithm is **simple algorithm** capable of **clustering** this **dataset** *quickly / efficiently* in just a **just a few iterations** 

---

- Train a **k-means clusterer** on the *dataset*
  - Tries to **find** each **blobs center** and *assign* each **instance** to the **closest blob**. 

```python
from sklearn.cluster import KMeans
k = 5

kmeans = KMeans(n_clusters=k)

y_pred = kmeans.fit_predict(X)
```

- Note $\to$ must specify the **number of clusters** $k$ that the **algorithm** must find
  - Clearly 5 here but not always obvious as stated soon.

---

- Each **instance** was **assigned** to one of the **5 clusters**

  - In the **context** of **clustering** $\to$ an *instances* **labels** is the **index** of the **cluster** 

    - with this *instance* gets assigned by the **algorithm**.
- This is **not to be confused** with the **class labels** in *classification* 
  
  - Recall that **clustering** is an **unsupervised learning task**

---

- The `Kmeans` instance preserves a **copy** of the **labels** of the **instances** it was **trained on**
  - Available via the `labels_` instance variable.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211221012401246.png)

- Look at **5 centroids** that the **algorithm located**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211221013356051.png)

- Easily assign **new instances** to the **cluster** whose **centroid is closest**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211221013435807.png)

- Plotting the **clusters decision boundaries**
  - Obtain a ***Voronoi tessellation***

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211221013557894.png)

- Most are correct but some are **misclassified** 
  - Notably in the boundary of top left and the central cluster.
- Does not **work well** with varying **diameters** of *blobs*
  - All the assignment considers is **distance** to the **centroid**

---

- Rather than **assigning each instance** to a *single cluster*
  - Called **==hard clustering==** $\to$ useful to **give each instance** a *score per cluster* called **==soft clustering==**. 
- Score can be the **distance** between the **instance** and the **centroid**
  - Alternatively $\to$ be some **similarity score** (*affinity score*) such as **gaussian radial basis** function

---

- In the `KMeans` $\to$ the `transform()` method measures the **distance** for **each instance** to *every centroid*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211221015203458.png)

- For this example $\to$ the **first instance** `X_new` distance are first row for 1st to 5th centroid.

---

- If you have a **high dimensional** *dataset* and you **transform it as such**
  - End up with a $k$ dimensional dataset
    - This can be **very efficient non linear** *dimensionality reduction technique.*

#### K Means algorithm

- Say we are **given the centroids**
  - Can easily **label all the instances** $\to$ in the *dataset* by **assigning** each of them to **cluster** whose **centroid** is *closest*.

---

- *Conversely* $\to$ if you were **given** all the **instance labels** 
  - You could locate all the **centroids** by *computing* the **mean** of the **instances** for **each cluster**. 

---

- We are given **neither** the **centroid / labels**
  - We start by **placing** *centroid* **randomly** (pick $k$ instances at **random** and using **their locations** as **centroids**) 
- We then *label* these *instances*
  - **Update** the **centroids** > label > update ... till the **centroids stop moving**.

---

- This is **guaranteed** to *converge* in a **finite number of steps** 
  - Often small, and will **never oscillate forever**.
- View algorithm below 
  - The **centroids** are **randomly init** (*top left*)
  - Label instances (*top right*)
  - Centroids updated (*center left*)
  - relabel instances (*center right*)

---

- After just **3 iterations** the **algorithm** has *reached* a **clustering** that seems **close to optimal**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211221035605651.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211221035654777.png)

- The **algorithm** is *guaranteed* to **converge**
  - It may **not converge** to *correct solution* as in to some *local optimum* rather than *global*.
- Depends on the **centroid initilization **  

---

- Example $\to$ Figure below shows **two sub optimal solutions** that the **algorithm** can **converge** to if you are **not lucky with the random init step.**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211221040055079.png)

- View some methods to **mitigate** this **risk** by **improving** the *centroid initliaization* 

#### Centroid initialization methods

- If you **approximately** *know* where to place the **centroids**
  - Such as **running** *clustering* algorithm earlier
    - Set the **init hyperparameter** to a `NumPy` array containing the **list of centroids**, set `n_init` to $1$. 

```python
good_init = np.array([[-3, 3], [-3, 2], [-3, 1], [-1, 2], [0, 2]])
kmeans = KMeans(n_clusters=5, init=good_init, n_init=1)
```

- Alternative solution
  - Running the **algorithm multiple times** with *various random inits* and **select the best solution**
    - Controlled by the `n_init` *hyperparameter*.
      - This is initially set to $10$ after calling `fit()`. 

---

- To determine the **best model** $\to$ **==inertia==** 
  - This is the **mean squared distance** between *each instance* and its **closest centroid** 
    - Equivalent to about $223.3$ for the **model** on left of figure above, $237.5$ for the model on the right of figure above, $211.6$ for the model in figure 9-3.
- The `KMeans` class runs the algorithm `n_init` times
  - Selects the **lowest inertia model**
    - Of course figure 9-3 is the best thus.

- Access this value:

```python
kmeans.inertia_ # 211.59
```

- The `score` returns the **negative inertia** 
  - This is because **greater** is **better**.

---

- An extension to **k-means** is **K-Means**+\\+ 
  -  Introduces a **smart specialization step** to select centroids that are **distance from one another**
    - Makes the **algorithm** *less likely* to to **converge** to some **suboptimal solution**.
- Showed that the **additional computation** required for this step is worth it
  - Rapidly **reduces** the **number of runs** for the algorithm to locate the **optimal solution**

---

1. Take some centroid $c^{(1)}$ 

   -  Chosen **uniformly** at **random** from the *dataset*.
2. Take some **new centroid** $c^{(i)}$ 

   - Choosing an **instance** $x^{(i)}$ with **probability**: $D(x^{(i)})^2\Sigma^m_{j=1}D(x^{(j)})^2$ 
   - Where $D(x^{(i)})$ is the **distance** between the **instance** $x^{(i)}$ and the **closest centroid** that was **chosen already**.
   - This **probability distribution** ensures that **instances further** from the *already chosen* centroids **are much more likely** to be *selected as centroids* 
3. Repeat the previous step until all $k$ centroids have been selected.

---

- `KMeans` uses this method by **default**
  - Set `init` to `"random"` to make it normal, rarely done.

#### Accelerated K-means and Mini Batch K-means.

- Can accelerate the **algorithm** by avoiding **unrequired distance calculations**
  - Exploiting the **triangle inequality**
    - Where the *straight* line is always the **shortest**
  - Keep track of **lower / upper bounds** for distances between **instances / centroids**

- This is used as default for `KMeans` class
  - Set `algorithm = full` for normal method, unlikely required.

---

- Alternatively, rather than using the **full dataset** at *each iteration*
  - This algorithm can use **mini batches**
    - Moving the **centroids** just **slightly** at *each iteration* 
  - This **speeds** up teh **algorithm** by some factor 3/4 
  - Enables us to **cluster huge datasets** that **do not fit in memory.*
- *Sklearn* implements this in the `MiniBatchKMeans` class.
  - Just use this like `KMeans` 

```python
from sklearn.cluster import MiniBatchKMeans

minibatch_kmeans = MiniBatchKMeans(n_clusters=5)
minibatch_kmeans.fit(X)
```

- If this **dataset** does **not in memory**
  - Use the `memmap` class
    - As done for **incremental PCA** for previous chapter.
- Alternatively
  - Pass **one mini batch** at a time to `partial_fit()` method
    - Requires **more work** as we need to **initialize it multiple times.** 

---

- Mini batch K means faster than regular but **worse inertia**.
  - Notably with the **number of cluster increases**, view in figure 9-6.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211221054749912.png)

- Difference between the **two curves** remains **fairly  constant**
  - Becomes **more significant** as $k$ increases as **inertia** becomes **smaller / smaller**
    - However in the **plot** on the **right** so you can see that **mini batch K-means** is **much faster** regular **K-means**
      - This difference increases with $k$. 

#### Finding Optimal Number of clusters

- So far set **number** of *clusters* $k$ to $5$ as it was **obvious** by *looking at the data*
  - Generally, its **not obvious** 
    - May be bad if you **set it to the wrong vlaue**
      - Example below setting $k$ to $3$ or $8$ results in **farily bad models**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211221055427492.png)

- May say just **select model** with the **lowest inertia**
  - $k=3$ has a **greater inertia** than $k=5$ but $k=8$ is lower
    - The **inertia** is *not a good performance metric* when selecting $k$ as it gets **lower** as you **increase k** 
- The **more clusters**
  - The **closer each instance** is to its **closest centroid**
    - Therefore the **lower inertia**, plot this as a function of $k$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211221055628405.png)

- *Inertia* drops **rapidly** as you *increase* $k$ $\to$ 4
  - decreaes **slowly** beyond this point
    - Roughly forms an **arm shape** and we denote the **elbow point** as the **optimal value**.
      - Higher > no change really, lower > rapid change.
      - Higher could **split clusters for no reason**

---

- More expensive more precise method is the **==silhouette score==** 
  - This is the *mean* **==silhouette coefficient==** over **all the instances**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211221060135124.png)

- $a$ is the **mean distance** to the **other instances** within the **same cluster** (*mean intra cluster distance*)
- $b$ is the **mean nearest cluster distance** $\to$ mean distance to the **instances** of the **next closest cluster** 
  - Defined as the one that *minimizes $b$* excluding the **instances** the *instances own cluster*

---

- The *silhouette coefficient* varies from $-1$ to $+1$ 
- a coefficient close to +1 means that the **instance** is **well** **inside** its own **cluster** and **far** from other **clusters**, while a **coefficient** close to **0** means that it is **close** to a **cluster** **boundary**
- Finally a coefficient close to -1 means that the instance may have been assigned to the wrong cluster. 
- To compute the silhouette score, you can use Scikit-Learn’s  `silhouette_score()` function, giving it all the **instances** in the **dataset**, and the labels they assigned:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220207161606682.png)

- Compare the **silhouette scores** for *different numbers* of *clusters*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220207161633983.png)

- As you can see, this **visualization** is much **richer** than the previous one: 

  - in **particular**, although it **confirms that k=4** is a very **good** **choice**, it also underlines the **fact** that **k=5** is quite **good** as well, and **much** **better** than **k=6** or **7**. 
    - This was not **visible** when **comparing** inertias.
- An **even** more informative **visualization** is **obtained** when you **plot** every **instance’s silhouette coefficient**, **sorted** by the **cluster** they are **assigned** to and by the **value** of the **coefficient**. This is called a **silhouette** diagram

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220207161806459.png)

- The **vertical** **dashed** lines represent the **silhouette** **score** for each **number** of **clusters**. 
  - When **most** of the **instances** in a **cluster** have a **lower** **coefficient** than this score (i.e., if many of the instances stop short of the dashed line, ending to the left of it)
    - then the **cluster** is rather **bad** since this means its **instances** are **much** too close to other *clusters*.
- We can see that when **k=3** and when **k=6**, we get bad clusters. 
  - But when **k=4** or **k=5**, the clusters **look pretty good** – most **instances** **extend** beyond the **dashed** line, to the right and closer to 1.0.
    - When **k=4**, the **cluster** at **index** 1 (the third from the top), is rather **big**, while when **k=5**, all **clusters** have **similar** **sizes**, so even **though** the overall **silhouette** score from **k=4** is **slightly** greater than for **k=5**, it seems like a **good** **idea** to use **k=5** to get **clusters** of **similar** **sizes**.

### Limits of K means

- Despite its many merits, most notably being **fast** and **scalable**, K-**Means** is **not perfect**. 
-  It is **necessary** to run the **algorithm** **several** times to **avoid sub-optimal solutions**, plus you need to **specify** the **number** of **clusters**.
-  Moreover, K-**Means** does **not** **behave** very well when the **clusters** have **varying** sizes, **different** **densities**, or **non**-**spherical** shapes. 
  - For example, **Figure** **9**-**11** shows how **KMeans** **clusters** a **dataset** containing **three ellipsoidal clusters** of **different** **dimensions**, **densities** and **orientations**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220207181427906.png)

- As you can see, **neither of these solutions are any good**.
- The solution on the **left** is **better**, but it still chops **off** **25**% of the **middle** **cluster** and **assigns** it to the **cluster** on the **right**.
  -  The **solution** on the **right** is just **terrible**, even **though** its **inertia** is **lower**.
- Depending on **data** different **clustering** **algorithms** may **perform** better. For example, on these types of **elliptical clusters**, **Gaussian** mixture **models** work **great**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220207181613591.png)

- Look at few ways to benefit from clustering, we use K-means, but experiment with **other clustering algorithms**

###  EXTENSION Using clustering for image segmentation

- **==Image==** **==segmentation==** is the task of partitioning an image into multiple segments.
-  In **==semantic segmentation==**, all **pixels** that are **part** of the **same** **object** type **get** **assigned** to the **same segment**. 
  - For example, in a self-driving car’s vision system, all pixels that are part of a pedestrian’s image might be assigned to the “pedestrian” segment (there would just be one segment containing all the pedestrians).
- In **instance segmentation,** all **pixels** that are part of the same **individual object** are assigned to the **same segment**. 

    - In this case there would be a different segment for each pedestrian.
- The **state** of the **art** in **semantic** or **instance segmentation** today is achieved **using complex architectures** based on **convolutional neural networks** (see Chapter 14).
-  Here, we are going to do something **much simpler**: **==color segmentation==**.
  -  We will **simply assign pixels** to the **same segment** if they have a **similar color**. In **some applications**, this may **sufficient**, for **example** if you want to **analyze** **satellite** **images** to **measure** how much **total forest area** there is in a region, **color** **segmentation** may be just fine.

---

- Load the **image** using `imread()` function.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220207182140368.png)

- The image is represented as a **3D array**: 
  - First dimension’s size is the **height**, the second is the **width**, and the third is the number of **color channels**, in this case red, green and blue (RGB). 
    - In other words, **for each pixel** there is a **3D vector containing the intensities of red, green and blue**, each between 0.0 and 1.0 (or between 0 and 255 if you use imageio.imread()).
    -  Some **images may have less channels**, such as **grayscale** images (one channel), 
    - Or more channels, such as images with an **additional alpha channel for transparency**,
    -  Or **satellite** images which often **contain channels for many light frequencies** (e.g., infrared). 
    - The following code **reshapes** the **array** to get a **long list of RGB colors,** then it **clusters** these **colors** using **K-Means**.
    - For example, it may **identify** a **color cluster** for all **shades** of **green**. 
      - Next, for **each color** (e.g., dark green), it looks for the **mean color** of the **pixel’s color cluster**. 
        - For **example**, all **shades** of **green** may be **replaced** with the **same** **light** **green** color (assuming the mean color of the green cluster is light green). 
          - Finally it **reshapes** this **long** **list** of colors to get the **same** **shape** as the **original image**.

```python
X = image.reshape(-1, 3)
kmeans = KMeans(n_clusters=8).fit(X)
segmented_img = kmeans.cluster_centers_[kmeans.labels_]
segmented_img = segmented_img.reshape(image.shape)
```

