# Superdense Coding

> [[../QC MOC]] | Prev: [[Quantum Teleportation]] | Next: [[BB84 Quantum Key Distribution]]

---

## The Goal

Alice wants to send **2 classical bits** of information to Bob. But she can only send **1 qubit**.

Classically, 1 bit can carry 1 bit of information. That's it. But if Alice and Bob pre-share an entangled pair, Alice can send 2 classical bits by transmitting just 1 qubit. This is **superdense coding** — the inverse of teleportation.

---

## The Setup

Alice and Bob share a Bell pair $|\Phi^+\rangle = \frac{|00\rangle + |11\rangle}{\sqrt{2}}$.

Alice has qubit 1, Bob has qubit 2.

---

## The Protocol

Alice wants to send one of four messages: 00, 01, 10, 11. She applies one of four operations to **her qubit only**:

| Message | Gate Alice applies | Resulting joint state |
|---|---|---|
| 00 | $I$ (do nothing) | $\frac{\|00\rangle + \|11\rangle}{\sqrt{2}} = \|\Phi^+\rangle$ |
| 01 | $X$ (bit flip) | $\frac{\|10\rangle + \|01\rangle}{\sqrt{2}} = \|\Psi^+\rangle$ |
| 10 | $Z$ (phase flip) | $\frac{\|00\rangle - \|11\rangle}{\sqrt{2}} = \|\Phi^-\rangle$ |
| 11 | $XZ$ (both) | $\frac{\|10\rangle - \|01\rangle}{\sqrt{2}} = \|\Psi^-\rangle$ |

Alice has encoded 2 bits of information into which Bell state the pair is in — by only touching her own qubit.

She then **sends her qubit to Bob**.

---

## Bob Decodes

Bob now has both qubits. He applies CNOT (qubit 1 controls qubit 2), then Hadamard to qubit 1, then measures both:

| Bell state received | After CNOT + H | Measurement |
|---|---|---|
| $\|\Phi^+\rangle$ | $\|00\rangle$ | 00 |
| $\|\Psi^+\rangle$ | $\|01\rangle$ | 01 |
| $\|\Phi^-\rangle$ | $\|10\rangle$ | 10 |
| $\|\Psi^-\rangle$ | $\|11\rangle$ | 11 |

Bob recovers Alice's 2-bit message perfectly.

---

## Why This Works

The four Bell states are **orthogonal** — they can be perfectly distinguished by measurement. Alice's four operations map the initial Bell state to four different orthogonal states. Bob's CNOT+H circuit is the **Bell measurement** — it maps the four Bell states back to the four computational basis states.

The entangled pair acts as a shared resource that doubles the classical capacity of the quantum channel. Without the pre-shared entanglement, 1 qubit can carry at most 1 classical bit (Holevo's theorem).

---

## Teleportation vs Superdense Coding

These two protocols are duals of each other:

| | Teleportation | Superdense Coding |
|---|---|---|
| Goal | Send 1 qubit | Send 2 classical bits |
| Resource used | 1 ebit + 2 classical bits | 1 ebit + 1 qubit |
| Direction | Quantum state → classical + quantum | Classical bits → quantum |

Both consume one entangled pair (ebit). Together they show that 1 ebit + 1 qubit = 2 classical bits, and 1 ebit + 2 classical bits = 1 qubit.

---

> Next: [[BB84 Quantum Key Distribution]] | Up: [[../QC MOC]]
