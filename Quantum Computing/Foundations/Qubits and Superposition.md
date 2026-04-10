# Qubits and Superposition

> [[../QC MOC]] | Next: [[Quantum States and Dirac Notation]]

---

## Start Here: What is a Bit?

A classical bit is the atom of classical computing. It has exactly one value at any moment: **0 or 1**. A light switch. On or off. True or false. There is no in-between.

Everything your laptop does — every video, every calculation, every keystroke — is ultimately billions of these switches flipping.

---

## Now Forget That

A **qubit** (quantum bit) is the atom of quantum computing. And it behaves nothing like a classical bit.

A qubit can exist in a **superposition** — a state that is, in a very real physical sense, *both 0 and 1 at the same time*, until the moment you look at it.

This isn't a metaphor. It isn't "we don't know which one it is." The qubit genuinely has no definite value. It exists in a blend of both possibilities simultaneously.

---

## The Math of Superposition

Here's how we write a qubit's state:

$$|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$$

Let's unpack every symbol:

- $|\psi\rangle$ — pronounced "ket psi." This is just notation for "the state of the qubit." The $| \cdot \rangle$ brackets are called **Dirac notation** (more on this in the next note). Think of it as a label that says "this is a quantum state."

- $|0\rangle$ and $|1\rangle$ — the two **basis states**. These are the quantum equivalents of classical 0 and 1. In vector form:
$$|0\rangle = \begin{bmatrix}1\\0\end{bmatrix}, \qquad |1\rangle = \begin{bmatrix}0\\1\end{bmatrix}$$

- $\alpha$ and $\beta$ — the **amplitudes**. These are *complex numbers* (they can have a real and imaginary part). They tell you "how much" of each basis state is in the superposition.

So the full state $|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$ is literally just a vector:

$$|\psi\rangle = \alpha\begin{bmatrix}1\\0\end{bmatrix} + \beta\begin{bmatrix}0\\1\end{bmatrix} = \begin{bmatrix}\alpha\\\beta\end{bmatrix}$$

A qubit is a **2D complex vector**. That's it. All the quantum weirdness lives in what this vector means physically.

---

## The Normalisation Constraint

There's one rule the amplitudes must obey:

$$|\alpha|^2 + |\beta|^2 = 1$$

The $|\cdot|$ here means the **modulus** of a complex number: if $\alpha = a + bi$, then $|\alpha|^2 = a^2 + b^2$.

**Why this constraint?** Because $|\alpha|^2$ and $|\beta|^2$ turn out to be *probabilities* — the probability of getting 0 or 1 when you measure. And probabilities must sum to 1.

So the qubit state is a unit vector in a 2D complex space. Geometrically, it lives on the surface of a sphere (the **Bloch sphere**, more below).

---

## What Do the Amplitudes Mean?

Amplitudes are **not** probabilities. They are the *square roots* of probabilities — and crucially, they can be **negative** or **complex**.

When you measure a qubit:
- You get outcome $0$ with probability $|\alpha|^2$
- You get outcome $1$ with probability $|\beta|^2$

This rule — probability = squared amplitude — is called the **Born rule**. It's one of the fundamental postulates of quantum mechanics.

**Example:** If $\alpha = \frac{1}{\sqrt{2}}$ and $\beta = \frac{1}{\sqrt{2}}$, then:
$$|\alpha|^2 = \frac{1}{2}, \quad |\beta|^2 = \frac{1}{2}$$

This qubit is in a perfectly equal superposition — 50% chance of 0, 50% chance of 1. This is the state $|+\rangle$, and it's one of the most important states in quantum computing.

**Another example:** $\alpha = \frac{1}{\sqrt{2}}$, $\beta = -\frac{1}{\sqrt{2}}$. The probabilities are still 50/50. But the *sign* of $\beta$ is different. This is the state $|-\rangle$. It has the same measurement statistics as $|+\rangle$ — but it behaves completely differently when you apply gates to it. The sign (the **phase**) matters enormously for how states interfere with each other.

---

## Superposition is Not Ignorance

This is the most important conceptual point. When a qubit is in superposition, it is **not** secretly 0 or 1 and we just don't know which. The qubit has no definite value. This is experimentally verified — if it were secretly one or the other, Bell's theorem shows the universe would behave differently than it does.

The analogy: a coin spinning in the air is not heads or tails — it's genuinely in a superposition of both. The moment it lands (is "measured"), it becomes one or the other.

---

## The Bloch Sphere

Any single-qubit state can be written as:

$$|\psi\rangle = \cos\frac{\theta}{2}|0\rangle + e^{i\phi}\sin\frac{\theta}{2}|1\rangle$$

where $\theta \in [0, \pi]$ and $\phi \in [0, 2\pi)$. These two angles parameterise a point on a unit sphere — the **Bloch sphere**.

- North pole ($\theta = 0$): $|0\rangle$
- South pole ($\theta = \pi$): $|1\rangle$
- Equator ($\theta = \pi/2$): equal superpositions like $|+\rangle$, $|-\rangle$, $|i\rangle$, $|-i\rangle$

Applying a quantum gate = rotating this sphere. The Hadamard gate, for instance, rotates the sphere 180° around the axis halfway between X and Z.

---

## Why $n$ Qubits is Powerful

With $n$ qubits, the state lives in $\mathbb{C}^{2^n}$:

$$|\psi\rangle = \sum_{x=0}^{2^n-1} \alpha_x |x\rangle, \qquad \sum_x |\alpha_x|^2 = 1$$

- 1 qubit: 2-dimensional space
- 2 qubits: 4-dimensional space
- 10 qubits: 1024-dimensional space
- 50 qubits: $2^{50} \approx 10^{15}$-dimensional space — more dimensions than there are seconds since the Big Bang

A classical computer needs $2^n$ complex numbers to describe this state. A quantum computer just *is* this state — it's encoded in the physics of $n$ particles.

**But here's the catch:** you can't just read out all $2^n$ amplitudes. Measurement collapses the state to one outcome. The entire art of quantum algorithm design is engineering the amplitudes so that when you measure, the right answer comes out with high probability.

---

> Next: [[Quantum States and Dirac Notation]] | Up: [[../QC MOC]]
