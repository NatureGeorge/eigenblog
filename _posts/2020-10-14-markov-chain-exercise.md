---
title: Exercise of Markov Chain
author: Zefeng Zhu
date: 2020-10-14 10:30:40 +0800
categories: [Notes, Courses]
tags: [markov-chain, probability]
---

## Q1

Suppose you repeatedly does toss a fair coin and denote $T$ the first time you get three consecutive heads.

1. Compute $E[T]$
2. Verify your answer in [1] via simulation. You may use any programming language, but you have to attach your code. Specifically, repeta the following experiment for 100,000 times: uses a computer to simulate coin tosses and record the first time that you get three consecutive heads. Report the mean and standard deviation of the 100,000 recorded times.

> Hint: define a proper Markov chain with the state space {0, 1, 2, 3}.

### Markov Chain Model

Define State:

1. $\text{State}_{0}$: $0 \qquad\rightarrow$ zero consecutive head
2. $\text{State}_{1}$: $1 \qquad\rightarrow$ one consecutive heads
3. $\text{State}_{2}$: $2 \qquad\rightarrow$ two consecutive heads
4. $\text{State}_{3}$: $3 \qquad\rightarrow$ three consecutive heads

Define State Space $S = \{0, 1, 2, 3\}$

Define Initial State Probability Distribution:

$\{P(\text{State}_{0})=1, P(\text{State}_{1})=0, P(\text{State}_{2})=0,P(\text{State}_{3})=0\}$

&emsp;&emsp;&emsp;Set it as a $4\times1$ matrix $A$:

$$A=\begin{bmatrix}
       1\\
       0\\
       0\\
       0\\
\end{bmatrix}
$$

&emsp;&emsp;&emsp;Each row of the matrix denotes its corresponding state.

Define Transition Probability Matrix $Q$:

&emsp;&emsp;&emsp;Since we have:

$$
\begin{aligned}
    P(\text{State}_{0}|\text{State}_{0}) = 0.5, P(\text{State}_{1}|\text{State}_{0}) = 0.5, P(\text{State}_{2}|\text{State}_{0}) = 0.0, P(\text{State}_{3}|\text{State}_{0}) = 0.0\\
    P(\text{State}_{0}|\text{State}_{1}) = 0.5, P(\text{State}_{1}|\text{State}_{1}) = 0.0, P(\text{State}_{2}|\text{State}_{1}) = 0.5, P(\text{State}_{3}|\text{State}_{1}) = 0.0\\
    P(\text{State}_{0}|\text{State}_{2}) = 0.5, P(\text{State}_{1}|\text{State}_{2}) = 0.0, P(\text{State}_{2}|\text{State}_{2}) = 0.0, P(\text{State}_{3}|\text{State}_{2}) = 0.5\\
    P(\text{State}_{0}|\text{State}_{3}) = 0.0, P(\text{State}_{1}|\text{State}_{3}) = 0.0, P(\text{State}_{2}|\text{State}_{3}) = 0.0, P(\text{State}_{3}|\text{State}_{3}) = 1.0\\
\end{aligned}
$$

&emsp;&emsp;&emsp;shown as Finite State Machine:

<img src="../../assets/img/MC_FSM.svg">

&emsp;&emsp;&emsp;then:

$$Q=\begin{bmatrix}
       0.5&0.5&0.0&0.0\\
       0.5&0.0&0.5&0.0\\
       0.5&0.0&0.0&0.5\\
       0.0&0.0&0.0&1.0\\
\end{bmatrix}
$$

Set $n$ as the number of tosses, then we have:

$$
A_{n}=Q^{n}A
$$

&emsp;&emsp;&emsp;where $A_{n}$ denoted as the state distribution at $n$.

### Compute the Expectation

Set $e$ denoted as the expectation of additional times to toss from a particular state to three consecutive heads. Then we have:

$$
E=\begin{bmatrix}
       e_{\text{from 0 to 3}}\\
       e_{\text{from 1 to 3}}\\
       e_{\text{from 2 to 3}}\\
       e_{\text{from 3 to 3}}\\
\end{bmatrix}
=\begin{bmatrix}
       e_{0}\\
       e_{1}\\
       e_{2}\\
       e_{3}\\
\end{bmatrix}
$$

And Because:

$$
ME=E-\begin{bmatrix}
       1\\
       1\\
       1\\
       0\\
\end{bmatrix}
$$

So we have:

$$
\begin{aligned}
    0.5\cdot(e_0+e_1)&=e_0-1\\
    0.5\cdot(e_0+e_2)&=e_1-1\\
    0.5\cdot(e_0+e_3)&=e_2-1\\
    0&=e_3\\
\end{aligned}
$$

&emsp;&emsp;&emsp;so we get:

$$
\begin{aligned}
    e_3=0\\
    e_2=8\\
    e_1=12\\
    e_0=14\\
\end{aligned}
$$

then $E[T]=e_0=14$

### Simulation

#### Verify Expectation

```python
import numpy as np

def coin_flip(p=0.5):
    count = 1
    head = []
    while len(head) <3:
        if np.random.binomial(1, p):
            if len(head) == 0 or head[-1] + 1 != count:
                head = [count]
            else:
                head.append(count)
        count += 1
    return head[-1]

res = np.array([coin_flip() for _ in range(100000)])

print('mean:', res.mean())
print('std:', res.std())
```

```bash
mean: 14.03031
std: 11.925531070099142
```

#### Plot Value Distribution of T

<table>
    <tr>
        <td>
            <img src="../../assets/img/dist_of_b_3.svg">
        </td>
        <td>
            <img src="../../assets/img/box_of_b_3.svg">
        </td>
    </tr>
    <tr>
        <td>
            Dist Plot
        </td>
        <td>
            Box Plot
        </td>
    </tr>
</table>

#### Plot Probability Matrix

```python
import numpy as np
from pandas import DataFrame
import seaborn as sns
import matplotlib.pyplot as plt
plt.style.use('ggplot')

Q = np.array([
    [0.5, 0.5, 0, 0],
    [0.5, 0, 0.5, 0],
    [0.5, 0, 0, 0.5],
    [0, 0, 0, 1]
]).T

V = np.array([1,0,0,0])

def pipe(num):
    res_lyst = [np.matmul(Q, V)]
    for _ in range(num):
        res_lyst.append(np.matmul(Q, res_lyst[-1]))
    return np.array(res_lyst)

res = pipe(100)

df = DataFrame(res, columns=['P(State_%s)' % i for i in range(4)])

ax = df.plot()
ax.set_xlabel("number of tosses")
ax.set_ylabel("probability")

ax = sns.heatmap(res.T, cmap='viridis')
ax.set_xlabel("number of tosses")
ax.set_ylabel("probability")
```

<table>
    <tr>
        <td>
            <img src="../../assets/img/p_of_b_3.svg">
        </td>
        <td>
            <img src="../../assets/img/dp_of_b_3.svg">
        </td>
    </tr>
    <tr>
        <td>
            Dist Plot
        </td>
        <td>
            Box Plot
        </td>
    </tr>
</table>