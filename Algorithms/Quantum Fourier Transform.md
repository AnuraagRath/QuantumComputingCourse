# Quantum Fourier Transform

> [[../QC MOC]] | Prev: [[Grover's Algorithm]] | Next: [[Shor's Algorithm]]

---

## Start With the Classical DFT

The **Discrete Fourier Transform** (DFT) takes a sequence of $N$ complex numbers $x_0, x_1, \ldots, x_{N-1}$ and produces another sequence $X_0, X_1, \ldots, X_{N-1}$:

$$X_k = \frac{1}{\sqrt{N}}\sum_{j=0}^{N-1} x_j \, e^{2\pi i jk/N}$$

What does this do? It decomposes the sequence into its **frequency components**. If the original sequence has a repeating pattern with period $r$, the DFT will have large values at $k = 0, N/r, 2N/r, \ldots$ — the multiples of the fundamental frequency.

The **Fast Fourier Transform** (FFT) computes the DFT in $O(N \log N)$ operations. This is one of the most important algorithms in classical computing.

---

## The Quantum Version

The **Quantum Fourier Transform** (QFT) does the same thing, but on the **amplitudes** of a quantum state.

Given a quantum state $|\psi\rangle = \sum_{j=0}^{N-1} x_j|j\rangle$, the QFT produces:

$$\text{QFT}|\psi\rangle = \sum_{k=0}^{N-1} X_k|k\rangle, \qquad X_k = \frac{1}{\sqrt{N}}\sum_{j=0}^{N-1} x_j \, e^{2\pi i jk/N}$$

Same formula as the DFT — but acting on quantum amplitudes.

**The speedup:** The QFT uses $O(n^2)$ gates for $N = 2^n$ points. The classical FFT uses $O(N \log N) = O(n \cdot 2^n)$ operations. The QFT is **exponentially faster**.

**The catch:** You can't read out all $N$ Fourier coefficients $X_k$ — measurement collapses the state to one outcome. The QFT is useful as a **subroutine** that transforms the state into a form where measurement reveals global properties (like the period).

---

## The QFT on Basis States

The QFT maps each basis state $|j\rangle$ to:

$$\text{QFT}|j\rangle = \frac{1}{\sqrt{N}}\sum_{k=0}^{N-1} e^{2\pi i jk/N}|k\rangle$$

For $N = 2^n$, this has a beautiful **product form**. Writing $j$ in binary as $j = j_1 j_2 \ldots j_n$ (where $j_1$ is the most significant bit):

$$\text{QFT}|j\rangle = \frac{1}{\sqrt{2^n}}\bigotimes_{l=1}^{n}\left(|0\rangle + e^{2\pi i \, 0.j_{n-l+1}\ldots j_n}|1\rangle\right)$$

where $0.j_{n-l+1}\ldots j_n$ is the binary fraction $\frac{j_{n-l+1}}{2} + \frac{j_{n-l+2}}{4} + \ldots + \frac{j_n}{2^{l}}$.

**What this means:** Each output qubit independently encodes a different "bit" of the phase. The $l$-th qubit is in state $\frac{|0\rangle + e^{2\pi i \cdot 0.j_{n-l+1}\ldots j_n}|1\rangle}{\sqrt{2}}$ — a superposition with a phase that depends on the lower bits of $j$.

This factored structure is why the QFT circuit is efficient.

---

## The QFT Circuit

For 3 qubits ($N = 8$):

```
q0: ──[H]──[R2]──[R3]──────────────────[SWAP]──
           │     │                      │
q1: ───────●─────│──[H]──[R2]──────────│──────
                 │       │             │
q2: ─────────────●───────●──[H]────────[SWAP]──
```

Where $R_k = \begin{bmatrix}1&0\\0&e^{2\pi i/2^k}\end{bmatrix}$ is a controlled phase rotation.

**Reading the circuit:**
- Apply $H$ to q0 → creates superposition with phase $0.j_1$
- Apply controlled-$R_2$ (q1 controls, q0 target) → adds phase $0.0j_2$ to q0
- Apply controlled-$R_3$ (q2 controls, q0 target) → adds phase $0.00j_3$ to q0
- Repeat for q1 and q2
- Final SWAPs reverse the bit order (the QFT outputs bits in reverse order)

For $n$ qubits: $n$ Hadamards + $\frac{n(n-1)}{2}$ controlled phase gates = $O(n^2)$ gates total.

---

## Why the QFT Finds Periods

Suppose your quantum state is a periodic superposition with period $r$:

$$|\psi\rangle = \frac{1}{\sqrt{M}}\sum_{j=0}^{M-1}|x_0 + jr\rangle$$

(a superposition of $M$ equally spaced states, starting at $x_0$, with spacing $r$).

After the QFT, the amplitude of $|k\rangle$ is:

$$\frac{1}{\sqrt{M \cdot 2^n}}\sum_{j=0}^{M-1}e^{2\pi i (x_0 + jr)k/2^n} = \frac{e^{2\pi i x_0 k/2^n}}{\sqrt{M \cdot 2^n}}\sum_{j=0}^{M-1}e^{2\pi i jrk/2^n}$$

The sum $\sum_j e^{2\pi i jrk/2^n}$ is large (constructive interference) when $rk/2^n$ is close to an integer, i.e., when $k \approx m \cdot \frac{2^n}{r}$ for integer $m$.

So measuring after the QFT gives you a multiple of $\frac{2^n}{r}$. From this, you can extract $r$ using the **continued fractions algorithm** (a classical number theory tool).

This is exactly what [[Shor's Algorithm]] does.

---

## Connection to SVD and Eigendecomposition

The QFT diagonalises the **shift operator** $S|j\rangle = |j+1 \bmod N\rangle$. The eigenstates of $S$ are the QFT basis states, and the eigenvalues are the roots of unity $e^{2\pi i k/N}$.

This is the quantum analogue of how the DFT diagonalises circulant matrices — and how [[../../Mathematics of ML and DL/Foundations/Linear Algebra/SVD and PCA|SVD]] finds the natural basis of a data matrix. All three are "find the basis where the structure is diagonal."

---

> Next: [[Shor's Algorithm]] | Up: [[../QC MOC]]
