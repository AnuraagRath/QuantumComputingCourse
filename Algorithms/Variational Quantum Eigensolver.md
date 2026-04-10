# Variational Quantum Eigensolver (VQE)

> [[../QC MOC]] | Prev: [[Shor's Algorithm]] | Next: [[Quantum Approximate Optimization Algorithm]]

---

## The Problem: Ground State Energy

In quantum chemistry, the central question is: what is the lowest energy state (ground state) of a molecule? The ground state determines the molecule's structure, reactivity, and properties.

The energy of a quantum system is described by its **Hamiltonian** $H$ — a Hermitian matrix. The ground state energy is the **smallest eigenvalue** of $H$:

$$H|\psi_0\rangle = E_0|\psi_0\rangle, \qquad E_0 = \min_\psi \langle\psi|H|\psi\rangle$$

**Why is this hard classically?** For a molecule with $n$ electrons, the Hamiltonian lives in a $2^n$-dimensional Hilbert space. For $n = 50$ electrons, that's $2^{50} \approx 10^{15}$ dimensions — impossible to store, let alone diagonalise.

**Why is this a natural fit for quantum computers?** A quantum computer with $n$ qubits naturally lives in a $2^n$-dimensional space. It can represent and manipulate these states efficiently.

---

## The Variational Principle

The key mathematical fact underlying VQE:

> **For any state $|\psi\rangle$, the expectation value of $H$ is an upper bound on the ground state energy:**
> $$E_0 \leq \langle\psi|H|\psi\rangle$$

**Proof:** Expand $|\psi\rangle$ in the eigenbasis of $H$: $|\psi\rangle = \sum_k c_k|E_k\rangle$ where $H|E_k\rangle = E_k|E_k\rangle$ and $E_0 \leq E_1 \leq E_2 \leq \ldots$

$$\langle\psi|H|\psi\rangle = \sum_k |c_k|^2 E_k \geq E_0 \sum_k |c_k|^2 = E_0$$

(since $\sum_k |c_k|^2 = 1$ and $E_k \geq E_0$ for all $k$).

**Consequence:** If you can minimise $\langle\psi|H|\psi\rangle$ over all states $|\psi\rangle$, you find $E_0$. VQE does this by parameterising $|\psi\rangle$ with a quantum circuit and optimising the parameters classically.

---

## The Algorithm

### 1. Choose an Ansatz

An **ansatz** is a parameterised quantum circuit $U(\theta)$ that prepares a trial state:

$$|\psi(\theta)\rangle = U(\theta)|0\rangle^n$$

The circuit has tunable rotation gates, e.g.:

$$U(\theta) = \prod_{l=1}^{L}\left(\prod_i R_y(\theta_{li}) \cdot \text{CNOT layer}\right)$$

where $R_y(\theta) = e^{-i\theta Y/2} = \begin{bmatrix}\cos\theta/2 & -\sin\theta/2 \\ \sin\theta/2 & \cos\theta/2\end{bmatrix}$ is a rotation gate.

The ansatz must be expressive enough to reach the ground state, but shallow enough to run on noisy hardware.

### 2. Estimate the Energy

Run the circuit on the quantum computer to prepare $|\psi(\theta)\rangle$. Measure to estimate:

$$E(\theta) = \langle\psi(\theta)|H|\psi(\theta)\rangle$$

The Hamiltonian $H$ is decomposed into a sum of Pauli operators (always possible for any Hermitian matrix):

$$H = \sum_k h_k P_k, \qquad P_k \in \{I, X, Y, Z\}^{\otimes n}$$

Each term $\langle\psi|P_k|\psi\rangle$ is estimated by measuring in the appropriate basis. The total energy is the weighted sum.

### 3. Optimise Classically

Send $E(\theta)$ to a classical optimiser. The optimiser updates $\theta$ to reduce $E(\theta)$:

$$\theta \leftarrow \theta - \eta \nabla_\theta E(\theta)$$

This is exactly [[../../Mathematics of ML and DL/Foundations/Calculus/Gradient Descent|gradient descent]] — the quantum computer evaluates the loss function, the classical computer minimises it.

### 4. Repeat Until Convergence

The loop continues until $E(\theta)$ stops decreasing. The final $\theta^*$ gives the best approximation to the ground state energy.

---

## Computing the Gradient: Parameter Shift Rule

You can't backpropagate through a quantum circuit the way you do through a neural network. Instead, use the **parameter shift rule**:

$$\frac{\partial E}{\partial \theta_i} = \frac{E(\theta_i + \pi/2) - E(\theta_i - \pi/2)}{2}$$

This is an exact gradient (not an approximation) for circuits with rotation gates. It requires **2 circuit evaluations per parameter** — analogous to finite differences in [[../../Mathematics of ML and DL/Foundations/Calculus/Partial Derivatives and Gradients|numerical differentiation]], but exact.

For a circuit with $p$ parameters: $2p$ circuit evaluations per gradient step. This is the quantum analogue of backpropagation.

---

## VQE as a Neural Network

The analogy is precise:

| Neural Network | VQE |
|---|---|
| Parameterised layers $f(x; \theta)$ | Parameterised circuit $U(\theta)\|0\rangle$ |
| Forward pass | Circuit execution + measurement |
| Loss function $L(\theta)$ | Energy $E(\theta) = \langle\psi(\theta)\|H\|\psi(\theta)\rangle$ |
| Backpropagation | Parameter shift rule |
| Gradient descent | Classical optimiser (Adam, COBYLA, SPSA) |
| Vanishing gradients | Barren plateaus |
| Local minima | Local energy minima |

The key difference: the "model" is a quantum circuit, and the "forward pass" is a physical quantum computation. The loss landscape is determined by quantum mechanics, not by the architecture of a neural network.

---

## Barren Plateaus: The Vanishing Gradient Problem

For deep random circuits, the gradient $\frac{\partial E}{\partial \theta_i}$ vanishes **exponentially** with the number of qubits:

$$\text{Var}\left[\frac{\partial E}{\partial \theta_i}\right] \in O\left(\frac{1}{2^n}\right)$$

The loss landscape becomes exponentially flat — the optimiser can't find a direction to improve. This is the quantum analogue of the vanishing gradient problem in deep RNNs ([[../../Mathematics of ML and DL/DL Models/Recurrent Neural Network|RNNs]]).

Mitigations: problem-inspired ansätze (not random), layerwise training, gradient-free optimisers, noise-aware training.

---

> Next: [[Quantum Approximate Optimization Algorithm]] | Up: [[../QC MOC]]
