# Quantum States and Dirac Notation

> [[../QC MOC]] | Prev: [[Qubits and Superposition]] | Next: [[Quantum Gates]]

---

## Why Invent New Notation?

Quantum mechanics involves a lot of vectors, inner products, outer products, and tensor products. Writing them out in full matrix notation every time is tedious and cluttered. Paul Dirac invented a cleaner notation in the 1930s that physicists and quantum computing researchers use universally.

Once you learn it, it's actually faster and more intuitive than standard linear algebra notation. It's just a shorthand — there's no new math, only new symbols.

---

## Kets: Column Vectors

A **ket** $|\psi\rangle$ is a column vector representing a quantum state.

The two computational basis states are:

$$|0\rangle = \begin{bmatrix}1\\0\end{bmatrix}, \qquad |1\rangle = \begin{bmatrix}0\\1\end{bmatrix}$$

These are just the standard basis vectors $\mathbf{e}_1$ and $\mathbf{e}_2$ that you know from linear algebra. The $|\ \rangle$ notation just signals "this is a quantum state."

Any single-qubit state is a linear combination (superposition) of these:

$$|\psi\rangle = \alpha|0\rangle + \beta|1\rangle = \begin{bmatrix}\alpha\\\beta\end{bmatrix}$$

---

## Bras: Row Vectors (Conjugate Transpose)

A **bra** $\langle\psi|$ is the **conjugate transpose** of the ket $|\psi\rangle$:

$$\langle\psi| = |\psi\rangle^\dagger = \begin{bmatrix}\alpha^* & \beta^*\end{bmatrix}$$

where $\alpha^*$ means the complex conjugate of $\alpha$ (flip the sign of the imaginary part: if $\alpha = a+bi$, then $\alpha^* = a-bi$).

For the basis states:
$$\langle 0| = \begin{bmatrix}1 & 0\end{bmatrix}, \qquad \langle 1| = \begin{bmatrix}0 & 1\end{bmatrix}$$

---

## Bra-Ket: The Inner Product

Put a bra and a ket together and you get a **bracket** — an inner product. This is just the complex dot product:

$$\langle\phi|\psi\rangle = \phi^\dagger \psi = \sum_i \phi_i^* \psi_i$$

**Key properties of the basis states:**

$$\langle 0|0\rangle = \begin{bmatrix}1&0\end{bmatrix}\begin{bmatrix}1\\0\end{bmatrix} = 1 \qquad \text{(normalised)}$$

$$\langle 1|1\rangle = 1 \qquad \text{(normalised)}$$

$$\langle 0|1\rangle = \begin{bmatrix}1&0\end{bmatrix}\begin{bmatrix}0\\1\end{bmatrix} = 0 \qquad \text{(orthogonal)}$$

So $|0\rangle$ and $|1\rangle$ form an **orthonormal basis** — exactly like the standard basis vectors in $\mathbb{R}^2$.

**Normalisation condition** in bra-ket notation:

$$\langle\psi|\psi\rangle = |\alpha|^2 + |\beta|^2 = 1$$

---

## Outer Product: Building Matrices from Vectors

The **outer product** $|\psi\rangle\langle\phi|$ is a matrix. It's the quantum way to write projection operators and gates:

$$|0\rangle\langle 0| = \begin{bmatrix}1\\0\end{bmatrix}\begin{bmatrix}1&0\end{bmatrix} = \begin{bmatrix}1&0\\0&0\end{bmatrix}$$

$$|1\rangle\langle 1| = \begin{bmatrix}0\\1\end{bmatrix}\begin{bmatrix}0&1\end{bmatrix} = \begin{bmatrix}0&0\\0&1\end{bmatrix}$$

Notice: $|0\rangle\langle 0| + |1\rangle\langle 1| = I$ — the projectors onto the basis states sum to the identity. This is the **completeness relation**, and it's how you decompose any operator in a basis.

**What does $|0\rangle\langle 0|$ do to a state?**

$$|0\rangle\langle 0| \cdot |\psi\rangle = |0\rangle\langle 0|\psi\rangle = |0\rangle \cdot \alpha = \alpha|0\rangle$$

It projects the state onto $|0\rangle$ and scales by the amplitude $\alpha$. This is exactly a projection matrix — the same concept from [[../../Mathematics of ML and DL/Foundations/Linear Algebra/Eigenvalues and Eigenvectors]].

---

## Multi-Qubit States: Tensor Products

When you have two qubits, their joint state lives in a **4-dimensional** space. You build it using the **tensor product** $\otimes$.

For two qubits $|\psi_A\rangle = \begin{bmatrix}a\\b\end{bmatrix}$ and $|\psi_B\rangle = \begin{bmatrix}c\\d\end{bmatrix}$:

$$|\psi_A\rangle \otimes |\psi_B\rangle = \begin{bmatrix}ac\\ad\\bc\\bd\end{bmatrix}$$

You multiply every element of the first vector by the entire second vector.

**Example:** $|0\rangle \otimes |1\rangle$:

$$\begin{bmatrix}1\\0\end{bmatrix} \otimes \begin{bmatrix}0\\1\end{bmatrix} = \begin{bmatrix}1\cdot0\\1\cdot1\\0\cdot0\\0\cdot1\end{bmatrix} = \begin{bmatrix}0\\1\\0\\0\end{bmatrix}$$

We write this more compactly as $|01\rangle$ or $|1\rangle$ (interpreting the binary string $01$ as the integer 1).

**The four 2-qubit basis states:**

| State | Vector | Meaning |
|---|---|---|
| $\|00\rangle$ | $[1,0,0,0]^T$ | Both qubits are 0 |
| $\|01\rangle$ | $[0,1,0,0]^T$ | First 0, second 1 |
| $\|10\rangle$ | $[0,0,1,0]^T$ | First 1, second 0 |
| $\|11\rangle$ | $[0,0,0,1]^T$ | Both qubits are 1 |

For $n$ qubits: $2^n$ basis states, $2^n$-dimensional state space.

---

## Important Named States

These appear constantly in quantum algorithms. Worth memorising:

| Name | State | How to read it |
|---|---|---|
| $\|+\rangle$ | $\frac{1}{\sqrt{2}}(\|0\rangle + \|1\rangle)$ | Equal superposition, both amplitudes positive |
| $\|-\rangle$ | $\frac{1}{\sqrt{2}}(\|0\rangle - \|1\rangle)$ | Equal superposition, $\|1\rangle$ amplitude is negative |
| $\|i\rangle$ | $\frac{1}{\sqrt{2}}(\|0\rangle + i\|1\rangle)$ | Equal superposition, $\|1\rangle$ amplitude is imaginary |
| $\|\Phi^+\rangle$ | $\frac{1}{\sqrt{2}}(\|00\rangle + \|11\rangle)$ | Bell state — two entangled qubits |

The $|+\rangle$ and $|-\rangle$ states have identical measurement probabilities (50/50) but different phases. That phase difference is invisible when you measure — but it's everything when you apply more gates. This is why quantum computing is subtle.

---

## Why Complex Numbers?

You might wonder: why not just use real numbers for amplitudes? The answer is that complex amplitudes are what nature requires. The imaginary unit $i$ encodes **phase** — the "direction" of the amplitude in the complex plane. Phase is what enables destructive interference (see [[Interference]]), which is the mechanism behind quantum speedup.

If you restricted amplitudes to real numbers, you'd lose a huge class of quantum algorithms. The full power of quantum computing requires the complex plane.

---

> Next: [[Quantum Gates]] | Up: [[../QC MOC]]
