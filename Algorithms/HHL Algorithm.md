# HHL Algorithm

> [[../QC MOC]] | Prereqs: [[Quantum Phase Estimation]], [[../Algorithms/Quantum Fourier Transform]]

---

## The Problem: Solving Linear Systems

Given a Hermitian matrix $A \in \mathbb{R}^{N \times N}$ and a vector $\mathbf{b} \in \mathbb{R}^N$, find $\mathbf{x}$ such that:

$$A\mathbf{x} = \mathbf{b}$$

This is one of the most fundamental problems in all of science and engineering. It appears in:
- Solving differential equations (finite element methods)
- Machine learning (linear regression: $\mathbf{x} = (A^TA)^{-1}A^T\mathbf{b}$)
- Fluid dynamics, circuit simulation, graph problems

**Classical best:** $O(N^{2.37})$ using fast matrix multiplication (Coppersmith-Winograd). For sparse matrices: $O(N\sqrt{\kappa})$ where $\kappa$ is the condition number.

**HHL:** $O(\log N \cdot \kappa^2 / \epsilon)$ — **exponential speedup in $N$**.

---

## The Quantum Encoding

HHL doesn't output $\mathbf{x}$ as a classical vector — it outputs a quantum state $|x\rangle$ proportional to $\mathbf{x}$:

$$|b\rangle = \sum_{i=0}^{N-1} b_i|i\rangle \xrightarrow{\text{HHL}} |x\rangle = \frac{A^{-1}|b\rangle}{\|A^{-1}|b\rangle\|}$$

You can then measure expectation values $\langle x|M|x\rangle$ for some observable $M$ — but you can't read out all $N$ components of $\mathbf{x}$ (that would take $O(N)$ measurements and kill the speedup).

---

## The Key Idea: Eigenvalue Inversion

Decompose $A$ in its eigenbasis: $A = \sum_j \lambda_j |u_j\rangle\langle u_j|$, so $A^{-1} = \sum_j \lambda_j^{-1}|u_j\rangle\langle u_j|$.

Expand $|b\rangle$ in the eigenbasis: $|b\rangle = \sum_j \beta_j|u_j\rangle$.

Then: $A^{-1}|b\rangle = \sum_j \frac{\beta_j}{\lambda_j}|u_j\rangle$.

HHL achieves this by:
1. Using [[Quantum Phase Estimation]] to write the eigenvalues $\lambda_j$ into an ancilla register
2. Applying a **controlled rotation** that maps $|\lambda_j\rangle|0\rangle \to |\lambda_j\rangle\left(\sqrt{1-C^2/\lambda_j^2}|0\rangle + \frac{C}{\lambda_j}|1\rangle\right)$
3. Uncomputing the eigenvalue register (inverse QPE)
4. Post-selecting on the ancilla being $|1\rangle$

After post-selection, the state is proportional to $\sum_j \frac{\beta_j}{\lambda_j}|u_j\rangle = A^{-1}|b\rangle$.

---

## The Circuit (High Level)

```
|0⟩^n  ──[QPE]──[Rotation]──[QPE†]──[M] (post-select |1⟩)
|b⟩    ──[    ]──[        ]──[    ]──
ancilla ──[    ]──[        ]──[    ]──
```

1. **QPE** on $|b\rangle$ using $e^{iAt}$ as the unitary → writes eigenvalues into ancilla
2. **Controlled rotation** on a single ancilla qubit: $R_y(2\arcsin(C/\lambda_j))$
3. **Inverse QPE** to uncompute the eigenvalue register
4. **Measure** the rotation ancilla — post-select on $|1\rangle$

---

## The Caveats (Why HHL is Controversial)

The exponential speedup is real but comes with serious conditions:

| Condition | What it means |
|---|---|
| $A$ must be sparse | $O(\text{poly}(\log N))$ non-zero entries per row |
| $\kappa$ must be small | Condition number $\kappa = \lambda_{\max}/\lambda_{\min}$ must be $O(\text{poly}(\log N))$ |
| $\|b\rangle$ must be efficiently preparable | Loading classical data into a quantum state is hard (QRAM problem) |
| Output is a quantum state | Reading out all $N$ components takes $O(N)$ measurements — kills the speedup |

For many practical ML applications (e.g., linear regression), the data loading bottleneck alone eliminates the quantum advantage. HHL is most useful when you only need to compute a single expectation value $\langle x|M|x\rangle$, not the full solution vector.

---

## Connection to ML

HHL is the basis of several proposed quantum ML speedups:

- **Quantum linear regression:** Solve the normal equations $A^TA\mathbf{w} = A^T\mathbf{y}$ in $O(\log N)$ time
- **Quantum SVM:** The kernel matrix inversion in SVMs is a linear system
- **Quantum PCA:** Eigendecomposition via phase estimation on the covariance matrix

All of these have the same caveats — the speedup is real only if data loading is efficient and you only need expectation values, not the full solution.

---

> Up: [[../QC MOC]] | See also: [[../Quantum Complexity Theory]], [[../../Mathematics of ML and DL/ML Models/Support Vector Machines]]
