# Qiskit Simulators

> [[Qiskit MOC]] | Prev: [[Qiskit Basics — Circuits and Gates]] | Next: [[Running on Real Hardware]]

---

## Why Simulate?

Real quantum hardware is noisy, slow, and has limited qubit counts. For learning and development, you simulate on your laptop first. Qiskit provides several simulators, each modelling a different aspect of quantum computation.

---

## The Three Simulators You Need to Know

| Simulator | What it models | Use case |
|---|---|---|
| `StatevectorSimulator` | Perfect, noiseless quantum state | Algorithm development, debugging |
| `DensityMatrixSimulator` | Mixed states, decoherence | Noise analysis |
| `AerSimulator` (shot-based) | Measurement outcomes with sampling | Realistic results, noise models |

---

## 1. Statevector Simulator — Perfect Qubits

Tracks the full state vector $|\psi\rangle \in \mathbb{C}^{2^n}$ exactly. No noise, no sampling error.

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

qc = QuantumCircuit(3)
qc.h(0)
qc.cx(0, 1)
qc.cx(0, 2)   # 3-qubit GHZ state

sv = Statevector(qc)
print(sv)
# [0.707, 0, 0, 0, 0, 0, 0, 0.707]
# = (|000⟩ + |111⟩)/√2

print(sv.probabilities_dict())
# {'000': 0.5, '111': 0.5}
```

**Limitation:** Memory scales as $2^n$ complex numbers. 30 qubits = 16 GB RAM. 50 qubits = impossible classically.

---

## 2. Shot-Based Simulation — Realistic Sampling

Simulates running the circuit many times ("shots") and counting outcomes. This is what real hardware does.

```python
from qiskit import QuantumCircuit, transpile
from qiskit_aer import AerSimulator

qc = QuantumCircuit(2, 2)
qc.h(0)
qc.cx(0, 1)
qc.measure_all()

simulator = AerSimulator()
compiled = transpile(qc, simulator)
job = simulator.run(compiled, shots=1024)
result = job.result()

counts = result.get_counts()
print(counts)
# {'00': 512, '11': 512}  (approximately)
```

**Why shots matter:** Each shot is one run of the circuit. More shots = less statistical noise in your probability estimates. Standard: 1024–8192 shots.

---

## 3. Noise Simulation — Modelling Real Hardware

Add a noise model to simulate the errors that occur on real devices.

```python
from qiskit_aer import AerSimulator
from qiskit_aer.noise import NoiseModel, depolarizing_error, thermal_relaxation_error

# Build a noise model
noise_model = NoiseModel()

# 1% depolarising error on single-qubit gates
error_1q = depolarizing_error(0.01, 1)
noise_model.add_all_qubit_quantum_error(error_1q, ['h', 'x', 'y', 'z', 'rx', 'ry', 'rz'])

# 2% depolarising error on two-qubit gates
error_2q = depolarizing_error(0.02, 2)
noise_model.add_all_qubit_quantum_error(error_2q, ['cx'])

# Run with noise
noisy_sim = AerSimulator(noise_model=noise_model)
compiled = transpile(qc, noisy_sim)
result = noisy_sim.run(compiled, shots=1024).result()
print(result.get_counts())
# {'00': 490, '11': 498, '01': 18, '10': 18}  ← noise introduced wrong outcomes
```

---

## 4. Copying a Real Device's Noise Model

You can pull the actual noise model from a real IBM device:

```python
from qiskit_ibm_runtime import QiskitRuntimeService
from qiskit_aer.noise import NoiseModel

service = QiskitRuntimeService()
backend = service.backend('ibm_brisbane')

noise_model = NoiseModel.from_backend(backend)
sim = AerSimulator(noise_model=noise_model)
```

This lets you test your circuit against realistic hardware noise before submitting a real job.

---

## 5. Density Matrix Simulator

Tracks the full density matrix $\rho$ instead of a state vector. Required for mixed states and noise channels.

```python
from qiskit.quantum_info import DensityMatrix

qc = QuantumCircuit(2)
qc.h(0)
qc.cx(0, 1)

dm = DensityMatrix(qc)
print(dm)
# DensityMatrix([[0.5+0.j, 0. +0.j, 0. +0.j, 0.5+0.j],
#                [0. +0.j, 0. +0.j, 0. +0.j, 0. +0.j],
#                [0. +0.j, 0. +0.j, 0. +0.j, 0. +0.j],
#                [0.5+0.j, 0. +0.j, 0. +0.j, 0.5+0.j]])

print(dm.purity())  # 1.0 for pure state, < 1 for mixed
```

---

## Expectation Values

For VQE and QAOA, you need $\langle\psi|H|\psi\rangle$:

```python
from qiskit.quantum_info import Statevector, SparsePauliOp

qc = QuantumCircuit(2)
qc.h(0)
qc.cx(0, 1)

sv = Statevector(qc)

# Define observable H = Z⊗Z
H = SparsePauliOp('ZZ')
expectation = sv.expectation_value(H).real
print(expectation)  # 1.0 for |Φ+⟩ (both qubits always same)
```

---

> Next: [[Running on Real Hardware]] | Up: [[Qiskit MOC]]
