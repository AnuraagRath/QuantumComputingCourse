# Quantum Phase Estimation

> [[../QC MOC]] | Prereqs: [[../Algorithms/Quantum Fourier Transform]], [[../Foundations/Quantum Gates]]

---

## What It Does

Quantum Phase Estimation (QPE) solves this problem:

> Given a unitary $U$ and one of its eigenstates $|u\rangle$ (where $U|u\rangle = e^{2\pi i \varphi}|u\rangle$), find the phase $\varphi \in [0,1)$ to $n$ bits of precision.

This is one of the most important subroutines in quantum computing. It's the engine inside:
- **Shor's algorithm** (order finding is phase estimation on the modular exponentiation unitary)
- **HHL algorithm** (eigenvalue inversion requires knowing eigenvalues)
- **Quantum simulation** (energy levels are phases of time-evolution unitaries)
- **Quantum chemistry** (ground state energy = lowest eigenphase)

---

## Why Phases Encode Eigenvalues

If $U|u\rangle = e^{2\pi i \varphi}|u\rangle$, then $\varphi$ is directly related to the eigenvalue. For a Hamiltonian $H$ with eigenvalue $E$, the time-evolution operator $U = e^{-iHt}$ has eigenvalue $e^{-iEt}$, so $\varphi = \frac{Et}{2\pi}$. Measuring $\varphi$ gives you $E$.

---

## The Circuit

```
|0⟩ ──[H]──────────────────────────────[QFT†]──[M]
|0⟩ ──[H]──────────────────────────────        [M]
|0⟩ ──[H]──────────────────────────────        [M]
         │         │         │
|u⟩ ─────[U^4]───[U^2]─────[U^1]──────────────
```

**Top register:** $n$ ancilla qubits (here $n=3$), all initialised to $|0\rangle$
**Bottom register:** the eigenstate $|u\rangle$

---

## Step-by-Step

### Step 1: Hadamard on all ancilla qubits

Each ancilla goes to $|+\rangle = \frac{|0\rangle + |1\rangle}{\sqrt{2}}$.

### Step 2: Controlled-$U^{2^k}$ operations

The $k$-th ancilla qubit (counting from the bottom) controls $U^{2^k}$ applied to $|u\rangle$.

When the control qubit is $|1\rangle$, apply $U^{2^k}$ to $|u\rangle$:

$$U^{2^k}|u\rangle = e^{2\pi i \varphi \cdot 2^k}|u\rangle$$

By phase kickback, this phase kicks back onto the control qubit:

$$\frac{|0\rangle + e^{2\pi i \varphi \cdot 2^k}|1\rangle}{\sqrt{2}}$$

After all controlled operations, the $n$ ancilla qubits are in state:

$$\frac{1}{\sqrt{2^n}}\bigotimes_{k=0}^{n-1}\left(|0\rangle + e^{2\pi i \varphi \cdot 2^k}|1\rangle\right)$$

### Step 3: Recognise the QFT

Compare this to the QFT output formula from [[../Algorithms/Quantum Fourier Transform]]:

$$\text{QFT}|j\rangle = \frac{1}{\sqrt{2^n}}\bigotimes_{l=1}^{n}\left(|0\rangle + e^{2\pi i \cdot 0.j_{n-l+1}\ldots j_n}|1\rangle\right)$$

The ancilla register is exactly $\text{QFT}|\tilde{\varphi}\rangle$ where $\tilde{\varphi}$ is the $n$-bit binary representation of $\varphi$.

### Step 4: Apply Inverse QFT

Apply $\text{QFT}^\dagger$ to the ancilla register. This maps $\text{QFT}|\tilde{\varphi}\rangle \to |\tilde{\varphi}\rangle$.

### Step 5: Measure

Measuring the ancilla register gives $\tilde{\varphi}$ — the $n$-bit approximation of $\varphi$.

---

## Precision and Success Probability

With $n$ ancilla qubits, QPE estimates $\varphi$ to $n$ bits of precision (error $\leq 2^{-n}$) with probability $\geq 1 - \epsilon$ where:

$$n = \lceil \log_2(1/\epsilon) \rceil + \lceil \log_2(2 + 1/(2\delta)) \rceil$$

for desired precision $\delta$. More ancilla qubits = more precision.

If $\varphi$ is exactly representable in $n$ bits, QPE gives the exact answer with probability 1.

---

## QPE in Qiskit

```python
from qiskit import QuantumCircuit
from qiskit.circuit.library import QFT

def qpe_circuit(unitary, eigenstate, n_ancilla):
    n_target = eigenstate.num_qubits
    qc = QuantumCircuit(n_ancilla + n_target, n_ancilla)
    
    # Prepare eigenstate
    qc.compose(eigenstate, qubits=range(n_ancilla, n_ancilla + n_target), inplace=True)
    
    # Hadamard on ancilla
    qc.h(range(n_ancilla))
    
    # Controlled-U^(2^k)
    for k in range(n_ancilla):
        controlled_u = unitary.power(2**k).control(1)
        qc.compose(controlled_u, qubits=[k] + list(range(n_ancilla, n_ancilla + n_target)), inplace=True)
    
    # Inverse QFT on ancilla
    qc.compose(QFT(n_ancilla, inverse=True), qubits=range(n_ancilla), inplace=True)
    
    # Measure ancilla
    qc.measure(range(n_ancilla), range(n_ancilla))
    return qc
```

---

> Up: [[../QC MOC]] | Used in: [[../Algorithms/Shor's Algorithm]], [[HHL Algorithm]], [[Quantum Simulation]]
