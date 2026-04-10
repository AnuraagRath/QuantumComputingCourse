# Quantum Error Correction

> [[../QC MOC]] | Prev: [[Quantum Noise and Decoherence]]

---

## Why Classical Error Correction Doesn't Work

Classical computers correct errors by **redundancy**: store each bit three times (000 or 111), and if one flips, majority vote tells you the correct value.

Quantum error correction can't work this way, for two reasons:

1. **No-cloning theorem:** You cannot copy an unknown quantum state. $|\psi\rangle \to |\psi\rangle|\psi\rangle$ is forbidden by the linearity of quantum mechanics. (Proof: if cloning were possible, you could use it to send information faster than light via entanglement.)

2. **Measurement destroys superposition:** You can't check a qubit's value to see if it's been corrupted without collapsing it.

So how do you protect quantum information from errors without copying or measuring it?

---

## The Key Insight: Encode, Don't Copy

Instead of copying the qubit, you **spread its information across multiple entangled qubits** in a way that:
- Errors on individual qubits can be detected
- The logical qubit's state is never directly measured
- Errors can be corrected by measuring only the *correlations* between qubits (not the qubits themselves)

---

## The 3-Qubit Bit-Flip Code

The simplest example. Encode one logical qubit $|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$ into three physical qubits:

$$|0\rangle_L = |000\rangle, \qquad |1\rangle_L = |111\rangle$$

So $|\psi\rangle_L = \alpha|000\rangle + \beta|111\rangle$.

**Encoding circuit:** Apply CNOT from the first qubit to the second and third:

$$(\alpha|0\rangle + \beta|1\rangle)|00\rangle \xrightarrow{\text{CNOT}_{12}, \text{CNOT}_{13}} \alpha|000\rangle + \beta|111\rangle$$

**If a bit flip occurs on qubit 2:** $\alpha|000\rangle + \beta|111\rangle \to \alpha|010\rangle + \beta|101\rangle$

**Error detection without measuring the logical qubit:**

Measure the **parity** of pairs of qubits:
- Measure $Z_1 Z_2$: are qubits 1 and 2 the same? (This doesn't reveal whether they're 0 or 1 — only whether they agree)
- Measure $Z_2 Z_3$: are qubits 2 and 3 the same?

| $Z_1Z_2$ | $Z_2Z_3$ | Error |
|---|---|---|
| $+1$ | $+1$ | No error |
| $-1$ | $+1$ | Qubit 1 flipped |
| $-1$ | $-1$ | Qubit 2 flipped |
| $+1$ | $-1$ | Qubit 3 flipped |

The measurement outcomes (called the **error syndrome**) tell you which qubit flipped, without revealing $\alpha$ or $\beta$. Apply $X$ to the flipped qubit to correct it.

---

## The 3-Qubit Phase-Flip Code

Phase flips ($Z$ errors) are corrected by the same idea, but in the Hadamard basis:

$$|0\rangle_L = |{+}{+}{+}\rangle, \qquad |1\rangle_L = |{-}{-}{-}\rangle$$

Measure $X_1 X_2$ and $X_2 X_3$ to detect phase flips.

---

## The Shor Code: Correcting Both

Peter Shor's 9-qubit code combines both codes to correct arbitrary single-qubit errors:

$$|0\rangle_L = \frac{(|000\rangle+|111\rangle)^{\otimes 3}}{2\sqrt{2}}, \qquad |1\rangle_L = \frac{(|000\rangle-|111\rangle)^{\otimes 3}}{2\sqrt{2}}$$

This encodes 1 logical qubit in 9 physical qubits and can correct any single-qubit error (bit flip, phase flip, or any combination).

---

## Stabiliser Codes

Modern quantum error correcting codes are described using the **stabiliser formalism**. A stabiliser code is defined by a set of Pauli operators $\{S_i\}$ (the stabilisers) such that the logical codewords are the $+1$ eigenstates of all stabilisers:

$$S_i|\psi\rangle_L = +1 \cdot |\psi\rangle_L \quad \forall i$$

Errors anticommute with some stabilisers, changing their eigenvalue from $+1$ to $-1$. Measuring the stabilisers (not the logical qubit) reveals the error syndrome.

The **surface code** is the leading candidate for fault-tolerant quantum computing. It encodes 1 logical qubit in $d^2$ physical qubits arranged on a 2D grid, and can correct errors on up to $\lfloor(d-1)/2\rfloor$ qubits. With $d=7$: 49 physical qubits per logical qubit, correcting up to 3 errors.

---

## The Resource Cost

| Code | Physical qubits per logical qubit | Errors correctable |
|---|---|---|
| 3-qubit bit-flip | 3 | 1 bit flip |
| Shor code | 9 | 1 arbitrary error |
| Surface code ($d=7$) | 49 | Up to 3 errors |
| Surface code ($d=31$) | 961 | Up to 15 errors |

For Shor's algorithm to break RSA-2048, you'd need ~4000 logical qubits and ~$10^7$ physical qubits. This is why fault-tolerant quantum computing is a long-term goal, not a near-term reality.

---

> Up: [[../QC MOC]]
