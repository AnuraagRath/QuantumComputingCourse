# Density Matrices and Mixed States

> [[../QC MOC]] | Prev: [[../Foundations/Measurement and Collapse]] | Needed for: [[../Foundations/Quantum Noise and Decoherence]], [[../Algorithms/Variational Quantum Eigensolver]]

---

## The Limitation of State Vectors

Everything so far has described quantum states as **pure states** — a single state vector $|\psi\rangle$. This works perfectly for ideal, isolated qubits.

But real qubits are never perfectly isolated. They interact with the environment. They decohere. And sometimes you simply don't know the exact state — you only know a probability distribution over possible states.

For all of these situations, the state vector is insufficient. You need the **density matrix**.

---

## Two Kinds of Uncertainty

There are two fundamentally different kinds of uncertainty in quantum mechanics:

**Quantum uncertainty (superposition):** The qubit is in state $\frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$. This is a definite quantum state — we know it exactly. The uncertainty in measurement outcomes is intrinsic to quantum mechanics.

**Classical uncertainty (mixed state):** We don't know if the qubit is in $|0\rangle$ or $|1\rangle$ — it's $|0\rangle$ with probability $\frac{1}{2}$ and $|1\rangle$ with probability $\frac{1}{2}$. This is ignorance, not quantum superposition.

These two situations have identical measurement statistics (50/50) but are physically different. The density matrix distinguishes them.

---

## The Density Matrix

For a **pure state** $|\psi\rangle$, the density matrix is the outer product:

$$\rho = |\psi\rangle\langle\psi|$$

For a **mixed state** — a classical probability distribution over pure states $\{|\psi_i\rangle\}$ with probabilities $\{p_i\}$:

$$\rho = \sum_i p_i |\psi_i\rangle\langle\psi_i|$$

**Example — pure state $|+\rangle$:**

$$\rho_{|+\rangle} = |+\rangle\langle+| = \frac{1}{2}\begin{bmatrix}1\\1\end{bmatrix}\begin{bmatrix}1&1\end{bmatrix} = \frac{1}{2}\begin{bmatrix}1&1\\1&1\end{bmatrix}$$

**Example — mixed state (50/50 classical mixture of $|0\rangle$ and $|1\rangle$):**

$$\rho_{\text{mixed}} = \frac{1}{2}|0\rangle\langle 0| + \frac{1}{2}|1\rangle\langle 1| = \frac{1}{2}\begin{bmatrix}1&0\\0&1\end{bmatrix} = \frac{I}{2}$$

Both have the same diagonal entries (measurement probabilities 50/50), but the off-diagonal entries differ:
- $\rho_{|+\rangle}$ has off-diagonal $\frac{1}{2}$ — these encode **quantum coherence** (the phase relationship between $|0\rangle$ and $|1\rangle$)
- $\rho_{\text{mixed}}$ has off-diagonal $0$ — no coherence, purely classical

---

## Properties of Density Matrices

Every valid density matrix satisfies:

1. **Hermitian:** $\rho = \rho^\dagger$ (so eigenvalues are real)
2. **Positive semidefinite:** $\langle\phi|\rho|\phi\rangle \geq 0$ for all $|\phi\rangle$ (eigenvalues $\geq 0$)
3. **Unit trace:** $\text{Tr}(\rho) = 1$ (probabilities sum to 1)

**Distinguishing pure from mixed:**

$$\text{Tr}(\rho^2) = \begin{cases}1 & \text{pure state} \\ < 1 & \text{mixed state}\end{cases}$$

For the maximally mixed state $\rho = I/2$: $\text{Tr}(\rho^2) = \text{Tr}(I/4) = \frac{1}{2}$.

---

## Expectation Values and Measurement

The expectation value of an observable $O$ in state $\rho$:

$$\langle O \rangle = \text{Tr}(\rho O)$$

For a pure state $\rho = |\psi\rangle\langle\psi|$: $\text{Tr}(|\psi\rangle\langle\psi|O) = \langle\psi|O|\psi\rangle$ — recovers the familiar formula.

The probability of measurement outcome $m$ (with projector $P_m$):

$$P(m) = \text{Tr}(P_m \rho)$$

After measurement, the state updates to:

$$\rho \to \frac{P_m \rho P_m}{\text{Tr}(P_m \rho)}$$

---

## How Gates Act on Density Matrices

A unitary gate $U$ transforms the density matrix as:

$$\rho \to U\rho U^\dagger$$

For a pure state: $U|\psi\rangle\langle\psi|U^\dagger = (U|\psi\rangle)(U|\psi\rangle)^\dagger$ — still pure.

Noise channels (non-unitary operations) also act on density matrices. For example, the **depolarising channel** with error probability $p$:

$$\mathcal{E}(\rho) = (1-p)\rho + \frac{p}{3}(X\rho X + Y\rho Y + Z\rho Z)$$

This takes a pure state and makes it mixed — it's the mathematical description of decoherence.

---

## Von Neumann Entropy

The quantum analogue of Shannon entropy:

$$S(\rho) = -\text{Tr}(\rho \log \rho) = -\sum_i \lambda_i \log \lambda_i$$

where $\lambda_i$ are the eigenvalues of $\rho$.

- Pure state: $S = 0$ (no uncertainty)
- Maximally mixed state $I/2^n$: $S = n$ (maximum uncertainty — $n$ bits)

Von Neumann entropy measures how "mixed" a state is — how much quantum information has been lost to the environment.

---

## Why This Matters in Qiskit

When you run a circuit on real IBM hardware, the output is not a pure state — it's a mixed state due to noise. Qiskit's `DensityMatrix` simulator models this exactly. Understanding density matrices lets you:
- Interpret noisy simulation results
- Design noise-aware VQE/QAOA
- Understand what quantum error mitigation is actually doing
- Compute entanglement measures (partial trace, entanglement entropy)

---

> Up: [[../QC MOC]]
