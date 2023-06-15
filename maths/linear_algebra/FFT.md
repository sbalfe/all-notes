# Fast Fourier Transform https://www.youtube.com/watch?v=h7apO7q16V0

#### Coefficient Representation

- Two polynomials and We wish to find the product

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027214514631.png)

- Represent polynomials in computers as a list of values

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027214559396.png)

- This is the **==coefficient representation==**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027214637725.png)



- Multiplying two polynomials of power $d$ gives $2d$ = $O(d^2)$ complexity.

---

#### Value representation

- Taking any **2 points** can **uniquely** represent some **line**

- This extends to the **idea** of **any degree**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027214934395.png)

- So $x^2$ can be represented with **3 points**, and so on.

- The **multiplication** of **polynomials** in **value representation** is much easier:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027215231805.png)

- Generate some value representation by **generating** the **correct number of points** to match how many values you need to match the **degree** when its **multiplied**
  - So this creates degree 4 above so represent with **5 points each**.

---

- Pipeline is conversion of **coefficient** > **value** > **multiply** > **coefficient** to obtain **final result**.
  - The **FFT** can do this **transfer** and **inverse** for back from **value representation.**

---

### Coefficient -> value 

- Have some polynomial:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027215728459.png)

- Pick $n$ random **x coordinates** , then calculate the **y**
  - Takes **O(d)** each time > thus **O(nd)** ending up finally as $O(d^2)$  

##### Even / Odd functions for optimisation

- Optimise the problem
  - Consider **8 points**, $P(x) = x^2$ 
    - Which values to select ????
      - Selecting **some positive number** already gives us the **value** of the **other one** , gives us nicely paired coordinates via this property $P(-x) = P(x)$ via **even functions**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027220134382.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027220145017.png)

- Same idea for **cubic** but with a **sign flipped** for the **y instead** , $P(-x) = -P(x)$ 
  - Turn **4 points** into **8 points** for the evaluation.

---

### Evaluation

- Take some polynomial:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027220407036.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027220438066.png)

- Split up into **odd** and **even** degree terms

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027220515430.png)

- We then take a **factor** of $x$ out of the **odd one** to create a **polynomial of even degrees**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027220615119.png)

- Allows us to rewrite the **values** evaluated at each point that are **positive/negative pairs**
  - Saves us **lots of computation.**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027220652449.png)

- The degree of each one is now **just 2** down from **5** or degree $\large \frac{n}{2} - 1$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027220902651.png)

- This leads to the concept of using **recursion** as we just evaluate these values again at our inputs original value but **squared** as of the **way we split up the odd/even**. 

---

- RECAPPING, we wish to:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027221245109.png)

- Evaluate the polynomial at $n$ points where each value of $n$ is a **positive negative pair**.
- We then **split** the **polynomial** into **two evaluations** into **odd** and **even** degree components.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027221432353.png)

- Split into another **pair of evaluations** which require only $\frac{n}{2}$ points to **evaluate**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027221508118.png)

- Calculate each **respective values** using the **relationship** of **positive and negative** paired points.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027221611619.png)

- This obtains us the **value representation of the original polynomial.**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027221640535.png)

- This is an $O(n \log n)$ algorithm as the **recursive sub problems** breaking into 2 pairs performed **n times**. 

  *Essentially like a **binary search** but you **repeat the binary search**.*

- Issue at question is when we evaluate the **squared value** they become **not + / - paired** anymore.

- We must **expand our domain** to now include **complex numbers**...

  - Set of complex numbers that enables each **set of points** to be **paired up in negatives**

  - Which set should we **use**.

---

#### Example

- Our polynomial is $P(x) = x^3 + x^2 - x -1 $ 
- Requires at **least** $n=4$ points.
  - Must be **positive negative pairs**, so write as such.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027222858048.png)

- These are **evaluated** both at the **next layer down** $x^2_1$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027222953489.png)

- Each set **of two points** determined **from one evaluation** at the **next layer down**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027223350561.png)

- At the bottom level it boils to this single value of $x^4$

  - Let $x_1=1$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027223517025.png)
  - This of course trickles down to **1**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027223640027.png)

  - What $x_2$ to select so when we square it becomes **-1**

    - Simply just $i$ and $-i$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027223743523.png)

  - Thus completing our **recursion**.
    - We just solved the **equation** $x^4=1$ 
      - In other words the **4th roots of unity**. are these values.

- Attempt to **generalise**
  - Degree $5$ polynomial, require **6 points**
    - Easier to work **powers of 2** so select $n=8$ 
- Select 4 pairs of **minus/negative pairs** such that every point when **raised to** the **power of** 8
  - Is **equivalent to one**.
    - This is the **8th roots of unity**.

- General formulae thus becomes
  - From a polynomial of **degree** $d$. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027224147272.png)

---

#### Why does the roots of unity work?

- The $N^{th}$ roots of unity = $z^n=1$ 
  - The **angle** around the **circumference** of the **unit circle** in **complex plane** is $\large \frac{2 \pi}{n}$.  
  - Use **eulers formulae ** to create a **compact representation** of this **concept**.
- Define $\large \omega = e^{\frac{2 \pi i}{n}}$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027224838792.png)

- This omega value allows us to **define individual roots of unity.**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027224911431.png)

- The squaring works as its just **nudging the angle** based on the **cosine and sin representation** of moving around the **circle**. 
- So we thus Evaluate the polynomial at:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027225039610.png)

---

##### Why this works specifically

- Two key properties

  1. All our **input pairs** are **positive/negative paired**
     - $\omega^{j+\frac{n}{2}}$ is the **corresponding pair** of **points.**

     - Intuitively think that adding **half the points** gets **you opposite way around** 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027225447636.png)

  2. For the **recursive step** , we will **square values**

     - Pass the points to **even / odd** degree polynomial

       - We end up with the $\frac{n}{2}$ roots of unity with the square of $n$ **roots of unity**. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027230553742.png)

     - Holds at **every level** until a **single point** is **achieved** and **returned** 

---

### Summary of the algorithm.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027230818852.png)

- Take in our **polynomial** as a **coefficient representation**
- Define $\large \omega = e ^{\frac{2 \pi i}{n}}$ as a **vector** of **roots of unity** length $n$  

1. Handle the **base case** where $n=1$ where $P(1)$ 
   - Just evaluate at a single point
2. For the **recursive case** we call the **FFT** *twice* 
   - One taking in **odd degree terms**
   - One taking in **even degree terms**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027231532345.png)

3. These each only must be evaluated at $\large \frac{n}{2}$ **roots of unity** now.
4. These recursive calls lead to the **corresponding value representations** 
   - Denoted $y_e$ for **even degree** , $y_o$ for odd.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027231803565.png)

5. **Combine** the **output** to obtain the **value representation** of the **original polynomial.**

6. Our **negative pair combination** must now take $\omega^j = x^j$ now so replace 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027232012232.png)

   - With this **relationship**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027232046443.png)

   - We also know $-w^j = w^{j+\frac{n}{2}}$, which represents the **aspect** where **roots of unity** that are **negative** of the **positive version** is just **the same root of unity** but shifted along $\large \frac{n}{2}$ times which is visualised as being on the **direct opposite side.**  

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027232215883.png)

   - The $j$ index of each of our $y_e$ and $y_o$ correspond to the **even / odd** values with an **input** of $j$:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027232444602.png)

   - This allows us to rewriter our **calculation above** as

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027232533082.png)

7. Final step is to **return** every **value** of the **polynomial** evaluated at the **nth roots of unity**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211027232721732.png)

```python
def FFT(P):
    # P - p[p_0,p_1, ... , p_(n-1)] coeff rep
    n = len(p) # assume n is a power of 2, can handle more though.
    
    # base case, a constant degree 0 value.
    if n == 1:
        return P
    
    # define omega
    w = e^((2 ** pi * i)/n)
    
    # split the input polynomial into odd and even degree terms
    P_e, P_o = [p_0,p_1, ... , p_(n-2)], [p_1,p_3, ... , p_(n-1)]
    
    # recusrively call each functiono the odd and even respectively
    # visualise as a branch, of half degrees of original polynomial
    y_e, y_o = FFT(P_e), FFT(P_o)
    
    # initilaise output list which is a value representation
    # [0,0,0] for n = 3 .
    y = [0] * n
    
    for j in range(n/2):
        y[j] = y_e[j] + w**jy_0[j]
        y[j+n/2] = y_e[j] - w**jy_0[j]
        
    # return our value representation.
    return y
   
    
```

- Inverse is just inversing the **DFT matrix**. 

#### Pseudocode follow through

- FFT of $3x^3 + 2x^2 + x + 0$ 
  - pass in as array $[0,1,2,3]$

1. n = 4
2. $\omega = i$ 
3. split into odd and even indices $y_e = [0,2]$ and $y_o = [1,3]$ 
4. Pass these into the **recursive chain** 
   - FFT([0,2]) = **rec_1** = $[2,-2]$ = $y_e$ 
   - FFT([1,3]) = **rec_2** = [4,-2] = $y_o$ 
5. Recursion have finished so computer final fft value
6. for loop in range index 0 and 1 
   - using $\omega = i$ 
   - y[0] = y_e[0] + $\omega^0$y_o[0] = 4 + 2 = 6
   - y[2] = y_e[0] - $\omega^0$y_o[0] = 4 - 2 = 2 
   - next loop index j = 1 therefore w  = i now so we will make complex numebrs
   - y[1] = y_e[1] + $i$y_o[1] = -2-2i, real = -2, imag = -2
   - y[3] = y_e[1] + $\omega^0$y_o[1] = -2-i(-2) = -2+2i, real = -2, imag = 2

##### **rec_1** , P = [0,2]

1. $n=2$

2. $w=-1$ 

3. $P_e = 0$ and $P_o$ = $2$ 

4. Recurse back and **hits** our **base case** as its **constants**

   - So we return from FFT(0) = 0, FFT(2) = 2

5. Thus $y_e = 0$ and $y_o$ = 2

6. And we **move forward** calculating bottom 

7. y = [0,0]

8. for loop once for n/2 times where n = 2 and you dont include the 1 in range 0 to 1

   - So index 0 is only needed

   - y[0] = $y_e[0] + (\omega)(y_o[0]) = 0 + (1)(2)$ = $-2$ 
   -   y[1] = $y_e[1] + (\omega)(y_o[1]) = 0 - (1)(2)$ = $2$ (1 is calculated from for loop index + the number of items)

9. Return the y value $[2,-2]$ 

**rec_2** , P = [1,3]

- Same as **rec_1** logic but replace **ye = 0 and 2** to **1 and 3**
- Following same **calculations it returns**
- [4,-2]
