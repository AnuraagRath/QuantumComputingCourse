# Entanglement

> [[../QC MOC]] | Prev: [[Measurement and Collapse]] | Next: [[Quantum Circuits]]

---

## The Strangest Thing in Quantum Mechanics

Einstein called it "spooky action at a distance" and spent years trying to prove it couldn't be real. He was wrong. Entanglement is real, experimentally verified, and one of the most powerful resources in quantum computing.

But it's also one of the most misunderstood. Let's build up to it carefully.

---

## Separable States: When Qubits are Independent

Two qubits are **separable** (not entangled) when their joint state can be written as a tensor product of two individual qubit states:

$$|\psi\rangle = |\psi_A\rangle \otimes |\psi_B\rangle$$

**Example:** $|00\rangle = |0\rangle \otimes |0\rangle$. Qubit A is definitely 0, qubit B is definitely 0. They're independent — knowing one tells you nothing new about the other.

**Another example:** $|+\rangle \otimes |0\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle) \otimes |0\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |10\rangle)$

Qubit A is in superposition, qubit B is definitely 0. Still separable — you can describe each qubit independently.

---

## Entangled States: When Qubits are Inseparable

An **entangled** state cannot be written as a product of individual qubit states. The most famous example is the **Bell state**:

$$|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$$

Try to factor this as $(\alpha|0\rangle + \beta|1\rangle) \otimes (\gamma|0\rangle + \delta|1\rangle)$:

$$= \alpha\gamma|00\rangle + \alpha\delta|01\rangle + \beta\gamma|10\rangle + \beta\delta|11\rangle$$

For this to equal $\frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$, we need:
- $\alpha\gamma = \frac{1}{\sqrt{2}}$ (coefficient of $|00\rangle$)
- $\alpha\delta = 0$ (no $|01\rangle$ term)
- $\beta\gamma = 0$ (no $|10\rangle$ term)
- $\beta\delta = \frac{1}{\sqrt{2}}$ (coefficient of $|11\rangle$)

From $\alpha\delta = 0$: either $\alpha = 0$ or $\delta = 0$.
- If $\alpha = 0$: then $\alpha\gamma = 0 \neq \frac{1}{\sqrt{2}}$. Contradiction.
- If $\delta = 0$: then $\beta\delta = 0 \neq \frac{1}{\sqrt{2}}$. Contradiction.

**It's impossible.** The Bell state cannot be factored. The two qubits are genuinely inseparable — they must be described together as a single 4-dimensional object.

---

## How to Create Entanglement

Two gates: Hadamard + CNOT.

Start with $|00\rangle$. Apply $H$ to the first qubit:

$$|00\rangle \xrightarrow{H \otimes I} \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle) \otimes |0\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |10\rangle)$$

Now apply CNOT (first qubit controls, second qubit is target):

$$\frac{1}{\sqrt{2}}(|00\rangle + |10\rangle) \xrightarrow{\text{CNOT}} \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle) = |\Phi^+\rangle$$

The CNOT flipped the second qubit only when the first was $|1\rangle$. The result: the two qubits are now correlated — they're always the same value, but neither has a definite value until measured.

---

## The Four Bell States

There are four maximally entangled 2-qubit states, called the **Bell basis**:

$$|\Phi^+\rangle = \frac{|00\rangle + |11\rangle}{\sqrt{2}}, \qquad |\Phi^-\rangle = \frac{|00\rangle - |11\rangle}{\sqrt{2}}$$

$$|\Psi^+\rangle = \frac{|01\rangle + |10\rangle}{\sqrt{2}}, \qquad |\Psi^-\rangle = \frac{|01\rangle - |10\rangle}{\sqrt{2}}$$

These four states are orthonormal and span the entire 2-qubit space. They're the "most entangled" states possible — measuring one qubit tells you everything about the other.

---

## What Entanglement Means Physically

When you measure one qubit of an entangled pair:
- The outcome is random (50/50 for a Bell state)
- But the other qubit's outcome is **instantly determined**, no matter how far apart they are

This is not faster-than-light communication — you can't use it to send information, because you can't control which outcome you get. But the correlations are stronger than anything possible with classical physics. This was proven by **Bell's theorem** (1964) and confirmed experimentally.

---

## Entanglement as a Computational Resource

Entanglement is not just a curiosity — it's a resource that quantum computers exploit:

**Quantum teleportation:** Transfer the state of a qubit using one entangled pair + 2 classical bits. The qubit state "teleports" — the original is destroyed and recreated elsewhere.

**Superdense coding:** Send 2 classical bits of information using just 1 qubit, if you share an entangled pair. Doubles the classical capacity of a quantum channel.

**Quantum error correction:** Encode one logical qubit across multiple entangled physical qubits. Errors on individual qubits can be detected and corrected without measuring the logical qubit (see [[Quantum Error Correction]]).

**Algorithm speedups:** Entanglement creates global correlations across all qubits simultaneously. Combined with [[Interference]], this allows quantum algorithms to explore the solution space in ways classical algorithms cannot.

---

## Entanglement vs Classical Correlation

It's tempting to think entanglement is just like classical correlation — like two gloves in separate boxes, where finding a left glove tells you the other is right. But it's fundamentally different.

Classical correlations are **pre-determined**: the gloves were always left and right, we just didn't know. Quantum entanglement is **not pre-determined**: the qubits have no definite values until measured. Bell's theorem proves this — any "hidden variable" theory that tries to explain entanglement classically makes different predictions than quantum mechanics, and experiments confirm quantum mechanics is right.

---

> Next: [[Quantum Circuits]] | Up: [[../QC MOC]]
