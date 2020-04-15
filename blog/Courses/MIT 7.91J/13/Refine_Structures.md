---
layout: default
date: 2020-04-15
author: Zefeng Zhu
---

# Refine Structures

## Restating question

Before using the potential energy functions to determine the best structure for two candidate structures given the sequence, one has to refine the structures:

* If one of the structures is the correct one:
  * Need to determine side chain conformations before calculating potential
* If better structure is only approximate:
  * Need to refine backbone and side chains first

## Methods for Refining Structures

* Energy minization
  * Move towards the local conformation
  * Fast compared the other two
  * restricted to local changes
* Molecular dynamics
  * tries to simulate the biological process
  * Computationally very intensive
* Simulated annealing
  * tries to shortcut the route to some of those global free energy minima by raising the temperature and sample all the space, and then cooling down so we trap a high probility conformation

### Energy Minization

#### Gradient Descent

one dimension: 

$$x_{i}=x_{i-1}-\varepsilon f'(x_{i-1})$$

N dimensions: 

$$\vec{x}_{1}=\vec{x}_{0}-\varepsilon\nabla U_{0}(\vec{x}_{0})$$

$$\nabla U=\left(\cfrac{\partial U}{\partial x_{1}},\ldots,\cfrac{\partial U}{\partial x_{n}}\right)$$

Force:

$$F=-\nabla U$$

$$\vec{x}_{1}=\vec{x}_{0}+\varepsilon F_{0}(\vec{x}_{0})$$

> each step is moving in the direction of the force

> Not guaranteed to get to the correct energetic structure

### Molecular Dynamics

* Seeks to simulate the motion of molecules
* Can escape local minima

$$x(t_i)=x(t_{i-1})+v(t_{i-1})\times(t_i-t_{i-1})$$

$$v(t_i)=v(t_{i-1})+\cfrac{F(t_{i-1})}{m}\times(t_i-t_{i-1})$$

$$v(t_i)=v(t_{i-1})-\cfrac{\nabla U(t_{i-1})}{m}\times(t_i-t_{i-1})$$

> Short simulations take tremendous computing resources

> Length of simulation and protocol determine radius of convergence


### Simulated Annealing

* At low temperature we cannot escape local minima
* At high temperature the kinetic energy exceeds the potential energy barrier
* This approach allows us to balance the need for speed and the need to be at high temperature

#### Step

* Start at high temperature
* Find most probable states
* Reduce temperature to trap these states

#### Metropolis Algorithm

* Goal: efficiently search a large conformation space
* Can be understood in terms of physical processes, but much more general
* Note the difference from molecular dtnamics:
  * Molecules move under physical forces but temperatures are far outside of normal range
  * A sampling method(a search strategy) not a simulation

Acceptance Criteria:

* Randomly choose neighboring state
  * Always accept moves that reduce potential
  * Go uphill (higher potential) based on odds ratio

$$\cfrac{P(S_{\text{test}})}{P(S_{n})}=\cfrac{e^{-E_{\text{test}}/\text{kT}}}{Z(T)}/\cfrac{e^{-E_{n}/\text{kT}}}{Z(T)}=e^{-(E_{\text{test}}-E_{n})/\text{kT}}$$

> Boltzmann equation

##### Full Algorithm

Iterate for a fixed number of cycles or until convergence:

1. Start with a system in state $S_n$ with energy $E_n$
2. Choose a neighboring state at random; we will call it the proposed state: $S_{\text{test}}$ with energy $E_{\text{test}}$
3. If $E_{\text{test}}\lt E_{n}$: $S_{n+1}=S_{\text{test}}$
4. Else set $S_{n+1}=S_{test}$ with probalility $P=e^{-(E_{\text{test}}-E_{n})/\text{kT}}$
   * otherwise $S_{n+1}=S_{n}$

> Noted that if $kT$ is very high, we almost always take the new state (higher potential). And this is what allows us to climb those energetic hills.

##### Full Algorithm (prob. version)

To identify minima given a probability function: $P(S)$

1. Start with a system in state $S_n$
2. Choose a neighboring state at random: $S_{\text{test}}$
3. Compute acceptance ratio $a=\cfrac{P(S_{\text{test}})}{P(S_n)}$
4. If $a\gt 1$: $S_{n+1}=S_{\text{test}}$
5. Else set $S_{n+1}=S_{\text{test}}$ with probability $a$ and $S_{n+1}=S_{n}$ with probability $1-a$

> Not specific to protein structure. Used to sample diverse probability distributions.

