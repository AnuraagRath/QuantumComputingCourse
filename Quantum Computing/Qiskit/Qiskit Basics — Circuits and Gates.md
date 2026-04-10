# Qiskit Basics — Circuits and Gates

> [[Qiskit MOC]] | Next: [[Qiskit Simulators]]

---

## Installation

```bash
pip install qiskit qiskit-aer qiskit-ibm-runtime
```

---

## The Core Objects

Everything in Qiskit starts with three objects:

```python
from qiskit import QuantumCircuit, QuantumRegister, ClassicalRegister

qr = QuantumRegister(3, name='q')   # 3 qubits, named 'q'
cr = ClassicalRegister(3, name='c') # 3 classical bits for measurement results
qc = QuantumCircuit(qr, cr)         # the circuit itself
```

Shorthand (most common):
```python
qc = QuantumCircuit(3, 3)  # 3 qubits, 3 classical bits
```

All qubits start in $|0\rangle$ by default.

---

## Applying Gates

Every gate from [[../Foundations/Quantum Gates]] has a direct Qiskit method:

```python
# Single-qubit gates
qc.h(0)        # Hadamard on qubit 0
qc.x(1)        # Pauli-X (NOT) on qubit 1
qc.y(0)        # Pauli-Y
qc.z(0)        # Pauli-Z
qc.s(0)        # S gate (phase π/2)
qc.t(0)        # T gate (phase π/4)
qc.sdg(0)      # S† (inverse S)
qc.tdg(0)      # T† (inverse T)

# Rotation gates (angle in radians)
qc.rx(theta, 0)   # Rotation around X-axis by theta
qc.ry(theta, 0)   # Rotation around Y-axis
qc.rz(theta, 0)   # Rotation around Z-axis
qc.p(theta, 0)    # Phase gate: |1⟩ → e^{iθ}|1⟩

# Two-qubit gates
qc.cx(0, 1)    # CNOT: qubit 0 controls qubit 1
qc.cz(0, 1)    # Controlled-Z
qc.swap(0, 1)  # SWAP

# Three-qubit gates
qc.ccx(0, 1, 2)   # Toffoli (CCNOT)
qc.cswap(0, 1, 2) # Fredkin (controlled SWAP)
```

---

## Measurement

```python
qc.measure(0, 0)              # measure qubit 0 into classical bit 0
qc.measure([0,1,2], [0,1,2])  # measure all at once
qc.measure_all()              # measure every qubit (adds classical register automatically)
```

---

## Visualising a Circuit

```python
qc.draw('mpl')    # matplotlib figure (best for notebooks)
qc.draw('text')   # ASCII art (works in terminal)
```

---

## A Complete Example: Bell State

```python
from qiskit import QuantumCircuit

qc = QuantumCircuit(2, 2)
qc.h(0)       # Hadamard on qubit 0 → superposition
qc.cx(0, 1)   # CNOT → entanglement
qc.measure([0, 1], [0, 1])

qc.draw('mpl')
```

This creates $|\Phi^+\rangle = \frac{|00\rangle + |11\rangle}{\sqrt{2}}$. When measured, you always get `00` or `11` — never `01` or `10`. That's entanglement.

---

## Inspecting the State Vector (Before Measurement)

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

qc = QuantumCircuit(2)
qc.h(0)
qc.cx(0, 1)

sv = Statevector(qc)
print(sv)
# [0.707+0.j, 0.+0.j, 0.+0.j, 0.707+0.j]
# = [α_00, α_01, α_10, α_11] = [1/√2, 0, 0, 1/√2]

sv.draw('qsphere')   # visualise multi-qubit state
```

---

## Parameterised Circuits (for VQE/QAOA)

```python
from qiskit.circuit import Parameter, ParameterVector

theta = Parameter('θ')
qc = QuantumCircuit(1)
qc.ry(theta, 0)

# Bind a specific value
bound_qc = qc.assign_parameters({theta: 1.57})

# Multiple parameters
params = ParameterVector('θ', length=4)
qc = QuantumCircuit(2)
qc.ry(params[0], 0)
qc.ry(params[1], 1)
qc.cx(0, 1)
qc.ry(params[2], 0)
qc.ry(params[3], 1)
```

---

## Getting the Unitary Matrix

```python
from qiskit.quantum_info import Operator

qc = QuantumCircuit(2)
qc.h(0)
qc.cx(0, 1)

op = Operator(qc)
print(op.data)  # the 4×4 unitary matrix of the full circuit
```

---

## Composing Circuits

```python
qc1 = QuantumCircuit(2)
qc1.h(0)

qc2 = QuantumCircuit(2)
qc2.cx(0, 1)

qc_full = qc1.compose(qc2)  # qc1 then qc2

qc.barrier()  # visual separator between sections
```

---

> Next: [[Qiskit Simulators]] | Up: [[Qiskit MOC]]
