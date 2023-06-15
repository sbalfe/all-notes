# Linear Maps

- We require **another vector space** say $W$ in addition to $V$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211209205823242.png)

## The vector space of Linear Maps

### Definition and Examples of Linear Maps

- Key definitions of *linear algebra* 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211209205953507.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211209210028852.png)

- Note that for **linear maps** $\to$ often use the **notation** $Tv$ as well as the **more standard notation** $T(v)$. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211209210203891.png)

- View some **examples** of *linear maps*
  - Ensure to **verify** that each of the **functions** defined below is a **linear map**

---

#### Zero map

- Along other uses
  - The symbol $0$ is used to **denote the function** that takes **each element** of some *vector space* to the **additive identity** of *another vector space*
    - Specifically $0 \in \mathcal{L}(V,W)$ defined by:

$$
0V=0
$$

- $0$ on the **LHS** is a *function* from $V$ to $W$ 
- $0$ on **RHS** is the **additive identity** in $W$ 
- As usual $\to$ the **context** should allow us to **distinguish** between the **various** uses of the $0$ symbol

#### Identity Map

- The *identity map*, denoted $I$ 
  - Is a **function** on *some vector space* that takes **each element to itself**
    - Specifically $I \in \mathcal{L}(V,V)$ defined by:

$$
Iv = v
$$

#### Differentiation Map

- Define $D \in \mathcal{L}(\mathcal{P}(\R), \mathcal{P}(\R))$ by:

$$
Dp = p'
$$

- The **assertion** that this function is a **linear map** is *another way* of stating a **basic result** on *differentiation*:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211209211045193.png)

- Whenever $f,g$ are **differentiable** and $\lambda$ is constant. 

#### Integration

- Define $T \in \mathcal{L}(\mathcal{P}(\R), \R)$ by:

$$
Tp = \int^1_0p(x)dx.
$$

- The **assertion** that this **function** is a *linear map* is another way of **static basic** result on **integration**
  - The integral of the **sum of two functions** equals the **sum of the integrals**
  - The integral of a **constant times a function equals** the *constant times the integral of the function*.

#### Multiplication by $x^2$ 

- Define $D \in \mathcal{L}(\mathcal{P}(R), \mathcal{P}(R))$ by:

$$
(Tp)(x) = x^2p(x)
$$

- For $x \in \R$

#### Backward shift

https://en.wikipedia.org/wiki/Shift_operator

- Recall $F^{\infty}$ denotes the **vector space** of *all sequences* of elements in $F$ 
  - Define $T \in \mathcal{L}(F^{\infty}, F^{\infty})$ by:

$$
T(x_1, x_2, x_3, \cdots) = (x_2, x_3, \cdots).
$$

#### From $\R^3$ to $\R^2$ 

- Define $T\in \mathcal{L}(\R^3, \R^2)$ by:

$$
T(x,z,y) = (2x -y + 3z, 7x + 5y-6z)
$$

#### From $F^n$ to $F^m$ 

- Generalizing the **previous example**
  - Let $m$ and $n$ be **positive integers** 
- Let $A_{i,k} \in F$  for $j=1,\cdots,m$ and $k=1 ,\cdots, n$ and define $T \in \mathcal{L}(F^n,F^m)$ by:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211209212856939.png)

- Actually **every linear map** from $F^n$ to $F^m$ is of this form.

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220125024826139.png)

##### Proof (Linear maps and basis of domain)

- Show the *existence* of said **linear map** $T$ 
  - Define $T: V \to W$ :

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220125025808357.png)

- $c_1, \cdots, c_n$ $\to$ **arbitrary elements** of $F$

  - The *list* $v_1, \cdots, v_n$ is some **basis** of $V$ thus above is some **function** from $V \to W$ 

    > Every *element* of $V$ may be **uniquely written** in some form $c_1v_1 + \cdots + c_nv_n$ )

- $\forall j$ , taking $c_j =1$ and other $c's = 0$ 
  -  Equation *above* shows that $Tv_j = w_j$. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220125030154072.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220125030350660.png)

- Thus $T$ is a **linear map** from $V \to W$ 
  - Must **prove uniqueness** 
    - Suppose $T \in \mathcal{L}(V,W)$ and $T_{v_j} = w_j$ for $j = 1 ,\cdots, n$ 
      -  Let $c_1, \cdots, c_n \in F$ 
    - The **homogeneity** of $T \implies T(c_jv_j) =c_jw_j \text{ for } j = 1 ,\cdots, n$  
    - The **additivity** of $T \implies$ :

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220125031216152.png)

- Thus $T$ is *uniquely determined* on $span(v_1 ,\cdots, v_n)$ via **the above equation**
  - As $v_1, \cdots, v_n$ is a **basis** of $V \implies$ $T$ is *uniquely determined* on $V$. 

## Algebraic operations on $\mathcal{L}(V,W)$ 

- Define **addition  / scalar multiplication** on $\mathcal{L}(V,W)$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220125031414611.png)

- We took **trouble** to *defined addition* and *scalar multiplication* on $\mathcal{L}(V,W)$ , this result should be **clear**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220221021703725.png)

- **Additive identity** of $\mathcal{L}(V,W)$ is the **zero linear map** defined earlier.
- Often **does not make sense** to *multiply two elements* of some **vector space** 
- Some pairs of **linear maps** of a **useful product exists**
- We require a **third vector space** thus suppose $U$ is a **vector space** *over* $F$.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220221021858947.png)

- Therefore its just **standard composition** $S \circ T$ of **two functions.**
- **Both linear** therefore just write $ST$.
- $ST$ defined only when $T$ maps into the **domain** of $S$.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220221022230810.png)

- **Multiplication** of linear maps is **not commutative** 
  - Thus $ST = TS$. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220221022539974.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220221023602037.png)

### Proof (Linear maps take 0 to 0)

- By additivity , we have

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220221023634984.png)

- Add the **additive inverse** of $T(0)$ to **each side** of the **equation** above to **conclude** that $T(0) = 0$ 