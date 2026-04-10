# BB84 Quantum Key Distribution

> [[../QC MOC]] | Prev: [[Superdense Coding]] | Up: [[../QC MOC]]

---

## The Problem: Secure Key Exchange

Two parties (Alice and Bob) want to establish a shared secret key over a public channel, without an eavesdropper (Eve) being able to learn it.

Classical cryptography solves this with mathematical hardness (Diffie-Hellman, RSA). But Shor's algorithm breaks these. **BB84** solves it with physics — the security comes from the laws of quantum mechanics, not computational hardness.

---

## The Key Quantum Fact

Measuring a qubit in the wrong basis **disturbs** it and gives a random result. If Eve intercepts a qubit and measures it, she inevitably introduces detectable errors. This is the security foundation of BB84.

---

## The Protocol

### Step 1: Alice Sends Qubits

Alice generates a random bit string and a random basis string. For each bit, she prepares a qubit:

| Bit | Basis | State sent |
|---|---|---|
| 0 | $+$ (computational) | $\|0\rangle$ |
| 1 | $+$ (computational) | $\|1\rangle$ |
| 0 | $\times$ (Hadamard) | $\|+\rangle$ |
| 1 | $\times$ (Hadamard) | $\|-\rangle$ |

She sends all qubits to Bob over a quantum channel.

### Step 2: Bob Measures

Bob randomly chooses a basis ($+$ or $\times$) for each qubit and measures.

- If Bob chose the **same basis** as Alice: he gets Alice's bit with certainty
- If Bob chose a **different basis**: he gets a random result (50/50)

### Step 3: Basis Reconciliation (Public)

Alice and Bob publicly announce which basis they used for each qubit (not the bits themselves). They **keep only the bits where their bases matched** — roughly 50% of the bits. This is the **raw key**.

### Step 4: Error Checking

They publicly compare a random subset of their raw key bits. If the error rate is above a threshold (~11%), they abort — an eavesdropper is present. If the error rate is low, they proceed.

### Step 5: Privacy Amplification

They apply a hash function to the remaining key to eliminate any partial information Eve might have. The result is a shorter but provably secure key.

---

## Why Eve Can't Hide

Suppose Eve intercepts each qubit, measures it, and resends a new qubit to Bob.

Eve doesn't know Alice's basis. She guesses correctly 50% of the time. When she guesses wrong:
- She measures in the wrong basis → gets a random result
- She resends a qubit in her (wrong) basis
- Bob measures in Alice's (correct) basis → gets a random result 50% of the time

**Error rate introduced by Eve:** For each qubit Eve intercepts, there's a 25% chance of an error in the final key (50% wrong basis × 50% wrong result). Over many qubits, this is easily detectable.

The no-cloning theorem prevents Eve from copying the qubit and measuring later — she must commit to a basis before knowing Alice's.

---

## Security

BB84's security is **information-theoretic** — it doesn't rely on computational hardness. Even an adversary with unlimited computing power (including a quantum computer) cannot break it without being detected.

This is fundamentally different from RSA or Diffie-Hellman, which are only computationally secure.

---

## In Qiskit

```python
from qiskit import QuantumCircuit
import numpy as np

def bb84_alice(bits, bases):
    """Prepare qubits according to BB84."""
    circuits = []
    for bit, basis in zip(bits, bases):
        qc = QuantumCircuit(1, 1)
        if bit == 1:
            qc.x(0)          # |1⟩
        if basis == 1:
            qc.h(0)          # rotate to Hadamard basis
        circuits.append(qc)
    return circuits
```

---

> Up: [[../QC MOC]]
