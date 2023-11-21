# Data oriented design

https://youtu.be/rX0ItVEVjHc

## Data oriented design principles

- Purpose of programs and each of their parts, is to transform data from one form to another.
- Understanding the problem by understanding the data.
- Different problem requires different solutions.
- Different data $\to$ different problem $\to$ different solution.
- No understanding of the cost of the solving the problem, we must not understand the problem.
- Solving problems you dont have creates more problems than you definitely do.

- Where there is one thing, there is likely to be many, try looking on the time axis.
- The more context, the better solution, do not throw away data you need.

## Three lies

### Software is a platform

- Hardware is the actual platform.
- Different hardware , different solutions.
- Idea of the right way is wrong

### Code designed around the model of the world

- Data hiding is implicit in world modelling.
- Creates maintenance problems $\to$ this allows changes to access.
- Creates problems around understanding properties of data, critical for our problem solving cases.

- World modelling implies some relationship to real data or transforms.
- Real life $\to$ classes are fundamentally similar, chair is a chair.
- Data transformations $\to$ show these classes are *superficially similar*.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20221002173554437.png)

- Very different the transformations on each, but the world modelling similarity brings this design into place.
- Leads to unrelated monolithic, unrelated data structures and transforms.
- World modelling attempts to idealise the problem.

### Code is more important that data

- Purpose of code is to transform data.
- Programmer responsible for the data, not the code
- code is the tool for said transformation.
- Understand data to understand the problem.
- There is no ideal abstract solution to the problem
- You can never future proof.
- Solve for transforming the data you have, given the constraints of the platform, nothing else.

## KV pair example.

- Dictionary lookup
- Code first $\to$ key value associated with one another in mental model.
- They are not actually associated, chance of needing a value for said key is one/ number of keys so very low chance.
- This is based on the data telling us this.
- Load value into cache but just throw it away, thus wasting bandwidth on the machine.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20221002174444352.png)

- Index position located first then passed to second system of getting values thus returning value.
- The cache is loaded with most likely data actually required.
- Cache miss for single cold value of value found.

---

- Solve for the most common case first, not the most generic.

## Memory Caching

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20221002175147519.png)

## L2 cache misses / frame

- Compilers are able to solve about 1-10% of the problem space.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20221002180109874.png)

- Align each input output struct to take advantage of the cacheline size to avoid cache misses and group cache-lines together
- Code is maintainable, debuggable and reason about the cost of change.
- uses full capacity of the L2 cache.
- 6 cache line reads are present, 32 iterations now for the count thus 6/32 = 5.33 loops per cacheline read.

---

## Common issues

### bools in structs

- Do not re read member values or re call functions when you already have the data
- hoist loop invariant reads and branches
- Even the super obvious ones that should already be in registers.
- not always bools.