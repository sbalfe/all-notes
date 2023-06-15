# Singular Value Decomposition

intro / intuition - https://www.youtube.com/watch?v=EokL7E6o1AE

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125130052459.png)

- See that it changes the basis to a new axis, and the singular values describe the scaling of the new principle axes that have been calculated.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125130303585.png)

- See how he has broken down the matrix A multiplying v using the other image intuition that it creates a new singular value and u as being the new axis and then converts into a multidimensional problem thus generating the new frame of reference to interpret this transformation as.
- To obtain the A in the original form simply take the conjugate transpose to solve for A matrix in SVD form.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125130639962.png)

- inverse of the unitary transformation obtain the original A in a new form of SVD
- Creates a diagonal matrix sigma in the middle with ordered singular values from top left to bottom right
- Final Value V is also called the idea of how these singular values and vectors are projected back to on to the original matrix A.

https://www.youtube.com/watch?v=gXbThCXjZFM&t=1s - alternative overview

https://www.youtube.com/watch?v=nbBvuuNVfco&list=PLMrJAkhIeNNSVjnsviglFoY2nXildDCcv&index=2 - mathematical overview 

https://www.youtube.com/watch?v=xy3QyyhiuY4&list=PLMrJAkhIeNNSVjnsviglFoY2nXildDCcv&index=3 - matrix approximation for pseudoinverse

https://www.youtube.com/playlist?list=PLMrJAkhIeNNSVjnsviglFoY2nXildDCcv - playlist for everything to do with SVD.

- important note that the v^T 
  - Its columns when v is actually transposed describes the mixture of U to add up to the original data matrix but scaled by the corresponding singular values.
- V ^T columns are mixtures of U values essentially but scaled by the singular values.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125164913659.png)

# PCA - Principle Component Analysis

https://www.youtube.com/watch?v=a9jdQGybYmE - best introduction and intuition

https://www.youtube.com/watch?v=fkf4IBRSeEc&list=PLMrJAkhIeNNSVjnsviglFoY2nXildDCcv&index=22 - alternative intro

## Application of SVD , face recognition and PCA

- Data matrix A with rows and columns
  - Must see which things are correlated together and which are redundant.
- We correlate the values using the SVD
  - As a method of **diagonalization** of the **covariance matrix** to obtain the **diagonal singular values** which give a *relative volatility* in other words **importance** for a **certain dimension** that we can **reduce too** to obtain more **specific information**.
- For the correlation matrix $C=A^*A$ , can do other way round but u must select from a different vector
  - Tells us how much information in each row is related to each other row
    - Has ideas of **redundant data** / **diagonalization** 
      - Diagonalization is important to obtain the **variance** of **specific vectors** of $A$ and this diagonalization is obtained via the **SVD** using the **singular vectors and singular values** and the **mapping** to the **original** matrix.

### Data for our face recognition

- Image > data matrix
  - Certain number of pixels in X and Y
  - where each location stores the colour thus just a number
- We wish to have many sets of images for each row of some matrix
  - Turn 2D array into a vector by reshape using matrix > vector conversion or **flattening the data**.
- Task is face recognition, stack some pictures of celebrities
  - Calculate the principle components of these celebrities.
    - Give some new picture of a celebrity $\to$ project onto our known images and **identify** who it is by the **strongest correlation**.

- Algorithm
  - read in the image as A.
  - grayscale it
  - make every image the same resolution
  - for every image of some face , average , as in 5 images of queen find the average matrix by adding and dividing by 5.
    - Extension into more complex CV techniques for specific complex movements in smiles/angles/etc.
  - reshape every average matrix into the rows of the data matrix, creating the SVD A matrix.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125133035913.png)

- From this matrix we observe $C=A^TA$ 

  - Correlation of the matrix with itself notice its a **dot product / inner product** of each image flattened row with one another.
    - Correlation of say image1 and image2 if different people such as obama and queen should be expected to not be as corelated.
      - But there are common features such as eyes/hair/nose etc thus denoting a possible similar correlation between each faces for this matrix.
        - This matrix will encapsulate this information for us.

- using the SVD

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125133708290.png)

- Extract three matrices from this matrix
  - Where $U$ = eigenvectors but more specifically the singular vectors 
  - With $V$ telling us how **each image** will *project* onto these various **eigenvectors**
- Calculate the **projection** onto these given a new image 
  - $\text{proj} = \alpha * V$ where $\alpha$ is the **new image** projected onto the **singular vector** with **singular value modes**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125135020477.png)

- shows the eigenfaces / (singular vectors of first matrix of SVD) against its corresponding importance i.e. singular value
- eigenfaces are a linear combination of the faces, where each column is taken in line for each 5 images.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125135133823.png)

- projection of faces onto clooneys eigenface space
  - Each person has a very different prediction
    - Essentially the encoding of the face or the key.,
- Now from the SVD project it backwards to obtain the average face of the projection

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125135620918.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125140348779.png)

- test relative distance to some image to calculate how similar they are
- takes the new image > calculates the projection and then reconstructs a new average face to say how similar it is to Margaret thatcher from the calculated face.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211125150403018.png)

---

### Eigenfaces

https://www.youtube.com/watch?v=ofWji_wQBEE - part 1 just go to other parts