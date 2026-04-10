# Quantum Circuits

> [[../QC MOC]] | Prev: [[Entanglement]] | Next: [[Interference]]

---

## The Circuit Model

A quantum circuit is the standard way to write down a quantum algorithm. It's a diagram that shows:
- Which qubits you're working with (horizontal wires)
- Which gates you apply (boxes on the wires)
- When you measure (meter symbols at the end)

Time flows left to right. Each wire carries one qubit's state through the computation.

---

## Reading a Circuit Diagram

```
q0: ──[H]──────●──────[M]
               │
q1: ───────────[X]────[M]
```

- `q0` and `q1` are two qubits, both starting in $|0\rangle$
- `[H]` on q0: apply the Hadamard gate to q0
- `●──[X]`: CNOT gate — q0 is the control (the dot), q1 is the target (the X)
- `[M]`: measure both qubits

This circuit creates a Bell state and then measures it.

---

## The Math Behind a Circuit

**Parallel gates** (applied to different qubits at the same time) use the **tensor product**:

$$U_{AB} = U_A \otimes U_B$$

For example, $H$ on q0 and $I$ (do nothing) on q1:

$$H \otimes I = \frac{1}{\sqrt{2}}\begin{bmatrix}1&1\\1&-1\end{bmatrix} \otimes \begin{bmatrix}1&0\\0&1\end{bmatrix} = \frac{1}{\sqrt{2}}\begin{bmatrix}1&0&1&0\\0&1&0&1\\1&0&-1&0\\0&1&0&-1\end{bmatrix}$$

**Sequential gates** (applied one after another) use **matrix multiplication**:

$$U_{\text{total}} = U_n \cdots U_2 U_1$$

Note the order: $U_1$ is applied first but written on the right (standard matrix convention).

The final state is:

$$|\psi_{\text{out}}\rangle = U_{\text{total}}|\psi_{\text{in}}\rangle$$

A full quantum circuit is just one big matrix-vector multiplication. The circuit is a recipe for building that matrix out of small, simple pieces.

---

## Worked Example: Bell State Circuit

Start: $|\psi_0\rangle = |00\rangle = \begin{bmatrix}1\\0\\0\\0\end{bmatrix}$

**Step 1:** Apply $H \otimes I$:

$$|\psi_1\rangle = (H \otimes I)|00\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |10\rangle) = \frac{1}{\sqrt{2}}\begin{bmatrix}1\\0\\1\\0\end{bmatrix}$$

**Step 2:** Apply CNOT:

$$|\psi_2\rangle = \text{CNOT}|\psi_1\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle) = \frac{1}{\sqrt{2}}\begin{bmatrix}1\\0\\0\\1\end{bmatrix}$$

That's the Bell state $|\Phi^+\rangle$. Two gates, two steps, done.

---

## Oracle Gates: Encoding a Problem

Many quantum algorithms use a special gate called an **oracle** $U_f$ that encodes a classical function $f$:

$$U_f|x\rangle|y\rangle = |x\rangle|y \oplus f(x)\rangle$$

Here $\oplus$ is XOR (addition mod 2). The oracle takes an input register $|x\rangle$ and an ancilla register $|y\rangle$, and XORs $f(x)$ into the ancilla.

**Why this form?** Because it's reversible — you can undo it by applying $U_f$ again. And it lets you evaluate $f$ on a superposition of all inputs simultaneously:

$$U_f\left(\sum_x \alpha_x|x\rangle\right)|0\rangle = \sum_x \alpha_x|x\rangle|f(x)\rangle$$

In one oracle call, you've computed $f$ on all inputs at once. This is **quantum parallelism** — but remember, you can't read out all the results. The art is using [[Interference]] to extract the useful information.

---

## Circuit Complexity Metrics

| Metric | What it measures | Why it matters |
|---|---|---|
| **Gate count** | Total number of gates | Proxy for runtime |
| **Circuit depth** | Longest path from input to output | Parallel gates reduce this; depth = time |
| **Width** | Number of qubits | Space requirement |
| **T-gate count** | Number of $T$ gates specifically | $T$ gates are expensive on fault-tolerant hardware |

**Depth is critical** because real qubits **decohere** — they lose their quantum state due to interaction with the environment (see [[Quantum Noise and Decoherence]]). A circuit must finish before decoherence destroys the computation. Shallower = faster = less decoherence.

---

## Circuit Identities (Useful Shortcuts)

Some gate combinations simplify:

$$H X H = Z \qquad \text{(Hadamard conjugates X to Z)}$$
$$H Z H = X \qquad \text{(and vice versa)}$$
$$H H = I \qquad \text{(Hadamard is its own inverse)}$$
$$\text{CNOT}^2 = I \qquad \text{(CNOT is its own inverse)}$$

These identities let you simplify circuits — fewer gates = shallower depth = less decoherence.

---

> Next: [[Interference]] | Up: [[../QC MOC]]
