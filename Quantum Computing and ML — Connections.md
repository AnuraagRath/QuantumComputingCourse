# Quantum Computing and ML — Connections

> [[QC MOC]] | See also: [[../Mathematics of ML and DL/🏠 Home]]

The math of quantum computing is not new math. It's the linear algebra, calculus, and probability you already know — applied to a different physical substrate. This note maps every major QC concept to something you've already studied.

---

## Linear Algebra is the Language of QC

| QC Concept | Math | ML Equivalent |
|---|---|---|
| Qubit state $\|\psi\rangle = \alpha\|0\rangle + \beta\|1\rangle$ | Unit vector in $\mathbb{C}^2$ | Weight vector in a neural network layer |
| $n$-qubit state | Unit vector in $\mathbb{C}^{2^n}$ | Embedding in a $2^n$-dimensional space |
| Quantum gate $U$ | Unitary matrix ($U^\dagger U = I$) | Weight matrix $W$ (but constrained to be unitary) |
| Applying a gate | Matrix-vector multiply $U\|\psi\rangle$ | Forward pass through a linear layer |
| Composing gates | Matrix multiplication $U_n \cdots U_1$ | Composing layers in a network |
| Measurement | Projection onto a basis vector | Argmax / sampling from a distribution |

→ [[../Mathematics of ML and DL/Foundations/Linear Algebra/Matrices and Operations]]
→ [[../Mathematics of ML and DL/Foundations/Linear Algebra/Eigenvalues and Eigenvectors]]

---

## The QFT is SVD's Cousin

The [[Algorithms/Quantum Fourier Transform]] diagonalises the shift operator — it finds the "natural frequency basis" of a periodic quantum state. This is the same idea as:

- **SVD**: finds the natural basis of a data matrix (directions of maximum variance)
- **PCA**: projects data onto the eigenvectors of the covariance matrix
- **FFT**: finds the frequency components of a classical signal

All of these are "find the basis where the structure is diagonal." The QFT just does it on quantum amplitudes, exponentially faster.

→ [[../Mathematics of ML and DL/Foundations/Linear Algebra/SVD and PCA]]

---

## Measurement is Probabilistic Inference

The Born rule $P(x) = |\langle x|\psi\rangle|^2$ is a probability distribution over outcomes. The quantum state $|\psi\rangle$ is a **probability amplitude distribution** — like a softmax output, but with complex numbers and interference.

| QC | ML |
|---|---|
| Amplitude $\alpha_x$ | Logit (pre-softmax score) |
| $\|\alpha_x\|^2$ | Probability after softmax |
| Measurement collapse | Sampling from a categorical distribution |
| Born rule | Softmax normalisation |

The key difference: amplitudes can be negative and complex, enabling destructive interference. Probabilities can't.

→ [[../Mathematics of ML and DL/Foundations/Statistics and Probability/Probability Basics]]
→ [[../Mathematics of ML and DL/Foundations/Statistics and Probability/Distributions]]

---

## VQE and QAOA are Gradient Descent on Quantum Circuits

[[Algorithms/Variational Quantum Eigensolver|VQE]] and [[Algorithms/Quantum Approximate Optimization Algorithm|QAOA]] are **variational algorithms**: parameterised quantum circuits trained by classical optimisers. The training loop is identical to ML:

```
Parameterised circuit U(θ)     ←→    Neural network f(x; θ)
Energy ⟨ψ(θ)|H|ψ(θ)⟩          ←→    Loss L(θ)
Parameter shift rule           ←→    Backpropagation
Classical optimiser (Adam etc) ←→    Same optimiser
Barren plateaus                ←→    Vanishing gradients
```

→ [[../Mathematics of ML and DL/Foundations/Calculus/Gradient Descent]]
→ [[../Mathematics of ML and DL/Foundations/Calculus/Partial Derivatives and Gradients]]
→ [[../Mathematics of ML and DL/DL Models/Recurrent Neural Network]] (vanishing gradients)

---

## Quantum Kernels and SVMs

A quantum computer can compute kernel functions $K(x, z) = |\langle\phi(x)|\phi(z)\rangle|^2$ where $|\phi(x)\rangle$ is a quantum feature map — a state prepared by a circuit encoding $x$.

This is the **kernel trick** from SVMs, but the feature space is exponentially large ($\mathbb{C}^{2^n}$) and the inner product is computed by the quantum computer in polynomial time.

→ [[../Mathematics of ML and DL/ML Models/Support Vector Machines]]

---

## Grover's Algorithm and Search in ML

[[Algorithms/Grover's Algorithm]] gives a quadratic speedup for unstructured search. In ML, this could accelerate:
- Hyperparameter search over a discrete space
- Nearest-neighbour search in a database
- Combinatorial optimisation (feature selection, architecture search)

The amplitude amplification technique generalises beyond search — it's a quantum analogue of **boosting**: repeatedly amplifying a weak signal (small amplitude on the right answer) into a strong one.

→ [[../Mathematics of ML and DL/ML Models/Gradient Boost]]

---

## Information Theory Connections

Quantum information theory extends classical information theory:

| Classical | Quantum |
|---|---|
| Shannon entropy $H = -\sum p \log p$ | Von Neumann entropy $S = -\text{Tr}(\rho \log \rho)$ |
| KL divergence | Quantum relative entropy |
| Mutual information | Quantum mutual information |
| Channel capacity | Quantum channel capacity |

The classical versions are what you use in decision trees, VAEs, and policy gradient methods. The quantum versions govern how much information can be transmitted through a quantum channel.

→ [[../Mathematics of ML and DL/Foundations/Statistics and Probability/Information Theory]]

---

## The Big Picture

```
Linear Algebra
  └─ Quantum states = unit vectors in ℂ^(2^n)
  └─ Gates = unitary matrices
  └─ QFT = quantum eigendecomposition → Shor's Algorithm

Probability & Statistics
  └─ Born rule = probability from amplitudes
  └─ Measurement = sampling
  └─ Quantum kernels → Quantum SVMs

Calculus & Optimisation
  └─ Parameter shift rule = quantum gradient
  └─ VQE / QAOA = gradient descent on circuits
  └─ Barren plateaus = vanishing gradients

Interference (uniquely quantum)
  └─ Amplifies correct answers
  └─ Cancels wrong answers
  └─ Enables exponential speedups (Deutsch-Jozsa, Shor)
```
