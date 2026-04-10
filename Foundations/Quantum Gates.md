# Quantum Gates

> [[../QC MOC]] | Prev: [[Quantum States and Dirac Notation]] | Next: [[Measurement and Collapse]]

---

## What is a Quantum Gate?

In classical computing, logic gates (AND, OR, NOT, XOR) take bits as input and produce bits as output. They're the building blocks of every classical circuit.

In quantum computing, **quantum gates** transform qubit states. They're the building blocks of quantum circuits.

But there's a critical difference: **every quantum gate must be reversible.** Given the output, you can always recover the input. Classical gates like AND are not reversible — if the output is 0, you can't tell if the input was (0,0), (0,1), or (1,0).

This reversibility requirement has a precise mathematical consequence: every quantum gate is a **unitary matrix**.

---

## What is a Unitary Matrix?

A matrix $U$ is **unitary** if:

$$U^\dagger U = I$$

where $U^\dagger$ is the **conjugate transpose** (transpose the matrix, then take the complex conjugate of every entry). $I$ is the identity matrix.

This means:
- $U^{-1} = U^\dagger$ — the inverse is just the conjugate transpose (easy to compute)
- $U$ **preserves norms**: $\|U|\psi\rangle\| = \||\psi\rangle\|$ — applying a gate never changes the total probability (it stays at 1)
- $U$ is a **rotation/reflection** in complex vector space — it moves the state around without stretching or shrinking it

Think of it like this: a unitary matrix is to complex vector spaces what an orthogonal matrix (rotation matrix) is to real vector spaces. It's a rigid transformation — no distortion, just rotation.

---

## Single-Qubit Gates

### The Pauli Gates

These are the three fundamental single-qubit gates, named after physicist Wolfgang Pauli.

**Pauli-X (the quantum NOT gate):**

$$X = \begin{bmatrix}0&1\\1&0\end{bmatrix}$$

It flips $|0\rangle$ to $|1\rangle$ and vice versa:
$$X|0\rangle = \begin{bmatrix}0&1\\1&0\end{bmatrix}\begin{bmatrix}1\\0\end{bmatrix} = \begin{bmatrix}0\\1\end{bmatrix} = |1\rangle$$
$$X|1\rangle = |0\rangle$$

On the Bloch sphere: a 180° rotation around the X-axis.

**Pauli-Z (the phase flip gate):**

$$Z = \begin{bmatrix}1&0\\0&-1\end{bmatrix}$$

It leaves $|0\rangle$ alone but flips the sign of $|1\rangle$:
$$Z|0\rangle = |0\rangle, \qquad Z|1\rangle = -|1\rangle$$

This doesn't change measurement probabilities at all — $|-1\rangle$ and $|1\rangle$ have the same $|\text{amplitude}|^2$. But the sign (phase) matters when the state interferes with other states. On the Bloch sphere: a 180° rotation around the Z-axis.

**Pauli-Y:**

$$Y = \begin{bmatrix}0&-i\\i&0\end{bmatrix}$$

Combines a bit flip and a phase flip. On the Bloch sphere: a 180° rotation around the Y-axis.

---

### The Hadamard Gate — The Most Important Gate in QC

$$H = \frac{1}{\sqrt{2}}\begin{bmatrix}1&1\\1&-1\end{bmatrix}$$

The Hadamard gate creates superposition from a definite state:

$$H|0\rangle = \frac{1}{\sqrt{2}}\begin{bmatrix}1&1\\1&-1\end{bmatrix}\begin{bmatrix}1\\0\end{bmatrix} = \frac{1}{\sqrt{2}}\begin{bmatrix}1\\1\end{bmatrix} = \frac{|0\rangle + |1\rangle}{\sqrt{2}} = |+\rangle$$

$$H|1\rangle = \frac{1}{\sqrt{2}}\begin{bmatrix}1&1\\1&-1\end{bmatrix}\begin{bmatrix}0\\1\end{bmatrix} = \frac{1}{\sqrt{2}}\begin{bmatrix}1\\-1\end{bmatrix} = \frac{|0\rangle - |1\rangle}{\sqrt{2}} = |-\rangle$$

**Why is this so important?** Almost every quantum algorithm starts by applying $H$ to all qubits. If you have $n$ qubits all in $|0\rangle$ and apply $H$ to each:

$$H^{\otimes n}|0\rangle^{\otimes n} = \frac{1}{\sqrt{2^n}}\sum_{x=0}^{2^n-1}|x\rangle$$

In one step, you've created a uniform superposition over all $2^n$ possible inputs. You've "loaded" all possible inputs simultaneously. This is the starting point for quantum parallelism.

**Hadamard is its own inverse:** $H^2 = I$. Apply it twice and you're back where you started. This is used in algorithms like [[../Algorithms/Deutsch-Jozsa Algorithm]].

---

### Phase Gates

$$S = \begin{bmatrix}1&0\\0&i\end{bmatrix}, \qquad T = \begin{bmatrix}1&0\\0&e^{i\pi/4}\end{bmatrix}$$

These leave $|0\rangle$ unchanged and rotate the phase of $|1\rangle$ by 90° ($S$) or 45° ($T$). They don't change measurement probabilities — they only affect phase. But phase is what controls interference, so these gates are essential for fine-tuning quantum algorithms.

The $T$ gate is particularly important: together with $H$ and CNOT, it forms a **universal gate set** — you can approximate any quantum computation with just these three gates.

---

## Two-Qubit Gates

### CNOT (Controlled-NOT)

$$\text{CNOT} = \begin{bmatrix}1&0&0&0\\0&1&0&0\\0&0&0&1\\0&0&1&0\end{bmatrix}$$

CNOT acts on two qubits: a **control** and a **target**.
- If the control is $|0\rangle$: do nothing to the target
- If the control is $|1\rangle$: apply $X$ (flip) to the target

$$\text{CNOT}|00\rangle = |00\rangle, \quad \text{CNOT}|01\rangle = |01\rangle$$
$$\text{CNOT}|10\rangle = |11\rangle, \quad \text{CNOT}|11\rangle = |10\rangle$$

**Why is CNOT so important?** It's the gate that creates **entanglement**. Apply $H$ to the first qubit, then CNOT:

$$|00\rangle \xrightarrow{H \otimes I} \frac{|00\rangle + |10\rangle}{\sqrt{2}} \xrightarrow{\text{CNOT}} \frac{|00\rangle + |11\rangle}{\sqrt{2}}$$

That final state is a Bell state — the two qubits are now entangled. You can't describe them independently anymore.

### Toffoli Gate (CCNOT)

Three qubits: two controls and one target. Flips the target only if **both** controls are $|1\rangle$. This is the quantum AND gate, and it's classically universal — any classical circuit can be built from Toffoli gates.

---

## Gates as Rotations on the Bloch Sphere

Every single-qubit gate is a rotation of the Bloch sphere. This gives you geometric intuition:

- $X$: 180° around X-axis (north pole ↔ south pole)
- $Z$: 180° around Z-axis (flips phase, doesn't move poles)
- $H$: 180° around the axis halfway between X and Z (swaps north pole with equator)
- $T$: 45° rotation around Z-axis

Composing gates = composing rotations. A sequence of gates traces a path on the Bloch sphere.

---

## Applying a Gate: It's Just Matrix Multiplication

$$|\psi'\rangle = U|\psi\rangle$$

A sequence of gates:

$$|\psi_{\text{final}}\rangle = U_n \cdots U_2 U_1 |\psi_0\rangle$$

This is identical to how layers in a neural network transform activations — except every $U$ must be unitary (reversible), not just any linear map. Neural networks can squash, stretch, and destroy information. Quantum gates cannot.

---

> Next: [[Measurement and Collapse]] | Up: [[../QC MOC]]
