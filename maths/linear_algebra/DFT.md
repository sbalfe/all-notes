# Discrete Fourier transform - 3b1b summary https://www.youtube.com/watch?v=g8RkArhtCc4&t=890s&ab_channel=TheJuliaProgrammingLanguage

https://www.youtube.com/watch?v=mkGsMWi_j4Q&t=297s&ab_channel=SimonXu

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211028033709281.png)

- List of **values** as the **setup** 

---

- Define them as a **list** $s$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211028033802659.png)

- First term of DFT is the **sum** of all **these numbers**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211028033840323.png)

- Like putting them **tip to tail** and saying how **long** is that **line**.

---

- Entering the complex plane
  - We use $\zeta$  to represent $e^{-2 \pi i / N}$  
    - This represents some **complex number** on the **unit circle.**
    - The **power** to $e$ is the **angle** in this case.
    - for our list this means  $e^{-2 \pi i / 8}$ , this translates to **clockwise 8th of a turn around circle.**  

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211028034011054.png)

- $\zeta$ at 0 is just **one** of course.
- Viewing **every zeta value**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211028034422018.png)

- So every value in our list is **moved around the circle** and the **value** it holds **scales** the **vectors** around the circle

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211028034535941.png)

- The input is **one** to mean we go around the circle **once**. $s[1]$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211028034647570.png)

- Example input **similar to that of a cosine wave.**
- The sum of the **weighted** rotations around the circle is **representing** where there **tips end up**.
- In that image the **signal is high**
- The arrows go round the **circle** **once** to represent the **1 cycle**. 
- It starts high > goes low then > high, it thus pulls some **low frequency term**.
- Take in **another input** with the **same period**
  - Has **same frequency** but a **different phase** such as **the sine wave**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211028035054340.png)

- This means the **complex /  real part is important.**

---

- Now onto the **second term** $s[2]$, do the **same thing** but we **walk faster around the circle**.
  - This represents **2 cycles** in of the **unit circle now**
    - So captures a **higher frequency sine wave for example**
- So if we **capture** some **large sum** in a **certain frequency** it has picked up this **frequency.**
  - If value at 2 is high there is a **frequency** in the **input** that represents **2 cycles** of the **unit circle**, value being the **summation** in the **range.**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211028050201109.png)

- So in $s[2]$ we **move around twice** the **time**
  - In this **double sin wave** values in the **same rotation** in each time appeared more then once so **created a large scalar value**
    - This creates **lobsided** and thus **specific frequency** responses to generate our **fourier transform.**
      - So like a **lock and key** where frequencies **match up** based on teh **summation of the vectors** that **each sample produces** when **rotated and scaled**, each frequency has a **fingerprint** that can be **scanned for** to see if its **present**.

---

- So the formulae 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211028035744462.png)

- Looks at various **frequencies** $f$ 
  - Looking at every **sample** $n$ 
- At each sample $s[n]$ we **multiply this scalar** to the **rotation around ** around the **unit circle** and **if it has a higher value** then its **picked up a spike** in the **sum** 
  - This is **viewed as the discrete Fourier transform**
- $f \cdot n$ $\to$ this parameter of $\zeta$ makes the **scalar values** walk **faster around the circle**
  - So at $f=3$ $\to$ you think it of as **going round 3 times**
    - The sum taken here **determines** the **magnitude** of the **list** at that **frequency** to determine the **exact frequency** of the input points.
- Basically as the **vectors magnitudes** becomes **focused** and **larger** they **are multiplied on top of each other more** creating spikes in the **Fourier transform.**

---

- Examples of signals :
  - Half of the **signal**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211028040443195.png)

- Looking at points to the **right and left** so its **equivalent** in the case of a **sine wave.**

---

# Step by step video

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211028041632817.png)

- White noise contains all frequencies so theoretically has a straight line
- Not in practice likely.
- Use complex notation to **not have two coefficients** to calculate.

---

https://www.youtube.com/watch?v=FjmwwDHT98c&t=1515s&ab_channel=gallamine - understanding Fourier transform and FFT intuitive.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211028042926230.png)

At 5 cycles/second, the signal frequency and winding frequency are aligned. This means all the high points of our signal happen on the right side of our circle; while all the low points are on the left. As a result, the center of mass of our wound signal is significantly skewed to the right, resulting in a non-zero energy! BOOM, we discovered the frequency that composed our signal!
