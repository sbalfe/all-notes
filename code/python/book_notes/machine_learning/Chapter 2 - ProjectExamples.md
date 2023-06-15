# Performance measures

### Root mean squared error

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117175223741.png)

- May prefer a different equation in some occasions.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117175538026.png)

- Both **RMSE** and **MAE** are measuring the **distance** between **two vectors**.
  - The *vectors* of **predictions** and **vector** of **target values**
    - Various **distance measures** or *norms* are *possible*.

---

> Computing the **root** of a *sum of squares* is the **Euclidean norm**
>
> The standard method of distance we are used to denoted $l_2$ **norm** $|| \cdot ||_2$ or $|| \cdot ||$  

---

> Computing the **sum of absolutes** is the $l_1$ **norm** denoted $|| \cdot ||_1$ 
>
> Called the **Manhattan norm** as it measures the **distance** between **two points** in a city 
>
> Travelling along **orthogonal city blocks only**

---

> General case the $l_k$ **norm vector** of a vector $v$ containing $n$ elements defined as
>
![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211117180121077.png)
>
> Gives the **number** of **non zero elements** in the vector and $l_\infty$ gives the **maximum absolute value** in the **vector**

- A **higher norm index** 
  - The more it **focuses** on **large values** and neglects **smaller values**
    - This is why **RMSE** is *more sensitive to outliers*.

