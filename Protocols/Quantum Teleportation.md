# Quantum Teleportation

> [[../QC MOC]] | Prev: [[../Foundations/Entanglement]] | See also: [[Superdense Coding]]

---

## The Goal

Alice has a qubit in an unknown state $|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$. She wants to send this quantum state to Bob, who is far away.

**The constraints:**
- She doesn't know $\alpha$ and $\beta$ (if she measured to find out, she'd collapse the state)
- She can't clone the qubit (no-cloning theorem)
- She only has a classical communication channel to Bob

**The solution:** Quantum teleportation. Using one pre-shared entangled pair and 2 classical bits, Alice can transfer the quantum state to Bob — perfectly, without ever knowing what the state is.

---

## The Setup

Before the protocol begins, Alice and Bob share a Bell pair $|\Phi^+\rangle = \frac{|00\rangle + |11\rangle}{\sqrt{2}}$.

- Alice has: qubit 1 (the unknown state $|\psi\rangle$) and qubit 2 (her half of the Bell pair)
- Bob has: qubit 3 (his half of the Bell pair)

Total initial state:

$$|\psi\rangle_1 \otimes |\Phi^+\rangle_{23} = (\alpha|0\rangle + \beta|1\rangle)_1 \otimes \frac{(|00\rangle + |11\rangle)_{23}}{\sqrt{2}}$$

Expanding:

$$\frac{1}{\sqrt{2}}\left(\alpha|000\rangle + \alpha|011\rangle + \beta|100\rangle + \beta|111\rangle\right)_{123}$$

---

## The Protocol

### Step 1: Alice applies CNOT (qubit 1 controls qubit 2)

$$\frac{1}{\sqrt{2}}\left(\alpha|000\rangle + \alpha|011\rangle + \beta|110\rangle + \beta|101\rangle\right)$$

### Step 2: Alice applies Hadamard to qubit 1

$$H \otimes I \otimes I: \quad |0\rangle \to \frac{|0\rangle+|1\rangle}{\sqrt{2}}, \quad |1\rangle \to \frac{|0\rangle-|1\rangle}{\sqrt{2}}$$

The state becomes:

$$\frac{1}{2}\Big[\alpha(|0\rangle+|1\rangle)|00\rangle + \alpha(|0\rangle+|1\rangle)|11\rangle + \beta(|0\rangle-|1\rangle)|10\rangle + \beta(|0\rangle-|1\rangle)|01\rangle\Big]$$

Regrouping by Alice's measurement outcomes (qubits 1 and 2):

$$\frac{1}{2}\Big[|00\rangle(\alpha|0\rangle + \beta|1\rangle) + |01\rangle(\alpha|1\rangle + \beta|0\rangle) + |10\rangle(\alpha|0\rangle - \beta|1\rangle) + |11\rangle(\alpha|1\rangle - \beta|0\rangle)\Big]$$

### Step 3: Alice measures qubits 1 and 2

She gets one of four outcomes (each with probability $\frac{1}{4}$), and Bob's qubit collapses to:

| Alice measures | Bob's qubit 3 | Correction needed |
|---|---|---|
| $\|00\rangle$ | $\alpha\|0\rangle + \beta\|1\rangle$ | None ($I$) |
| $\|01\rangle$ | $\alpha\|1\rangle + \beta\|0\rangle$ | Apply $X$ |
| $\|10\rangle$ | $\alpha\|0\rangle - \beta\|1\rangle$ | Apply $Z$ |
| $\|11\rangle$ | $\alpha\|1\rangle - \beta\|0\rangle$ | Apply $XZ$ |

### Step 4: Alice sends 2 classical bits to Bob

She tells Bob her measurement outcome (00, 01, 10, or 11). Bob applies the corresponding correction gate. His qubit is now exactly $|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$.

---

## What Just Happened?

- Alice's original qubit 1 is now in a definite classical state (it was measured and collapsed)
- Bob's qubit 3 is now in state $|\psi\rangle$ — the state has been "teleported"
- Alice never learned $\alpha$ or $\beta$ — the no-cloning theorem is not violated
- The state was not transmitted faster than light — Bob needs the 2 classical bits (which travel at most at light speed) to complete the protocol

**The entangled pair was the resource.** It was consumed in the process — you can't reuse it.

---

## The Circuit

```
q1 (|ψ⟩): ──────────[H]──●──[M]──────────────────
                          │   ↓ (classical bit)
q2 (Bell): ──●────────────[X]──[M]────────────────
             │                  ↓ (classical bit)
q3 (Bell): ──[X]───────────────────[X?]──[Z?]──
```

The $[X?]$ and $[Z?]$ are classically controlled corrections based on Alice's measurement results.

---

## In Qiskit

```python
from qiskit import QuantumCircuit, QuantumRegister, ClassicalRegister

qr = QuantumRegister(3)
cr = ClassicalRegister(2)
qc = QuantumCircuit(qr, cr)

# Prepare Bell pair on qubits 1,2
qc.h(1)
qc.cx(1, 2)

# Alice's operations
qc.cx(0, 1)
qc.h(0)

# Alice measures
qc.measure(0, 0)
qc.measure(1, 1)

# Bob's corrections (classically controlled)
qc.x(2).c_if(cr[1], 1)
qc.z(2).c_if(cr[0], 1)
```

---

> Next: [[Superdense Coding]] | Up: [[../QC MOC]]
