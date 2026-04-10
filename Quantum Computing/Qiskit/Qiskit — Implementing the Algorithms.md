# Qiskit — Implementing the Algorithms

> [[Qiskit MOC]] | Prev: [[Running on Real Hardware]]

This note implements every algorithm in the vault in actual runnable Qiskit code. Each implementation is minimal and annotated to connect directly to the math.

---

## Deutsch-Jozsa

See theory: [[../Algorithms/Deutsch-Jozsa Algorithm]]

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

def deutsch_jozsa(oracle: QuantumCircuit, n: int) -> str:
    """
    oracle: a QuantumCircuit on n+1 qubits implementing U_f
    Returns 'constant' or 'balanced'
    """
    qc = QuantumCircuit(n + 1, n)

    # Ancilla in |1⟩
    qc.x(n)

    # Hadamard on all qubits → uniform superposition + |−⟩ ancilla
    qc.h(range(n + 1))

    # Oracle (phase kickback encodes f into phases)
    qc.compose(oracle, inplace=True)

    # Second Hadamard on input register
    qc.h(range(n))

    # Measure input register
    qc.measure(range(n), range(n))

    # Simulate
    from qiskit_aer import AerSimulator
    from qiskit import transpile
    sim = AerSimulator()
    result = sim.run(transpile(qc, sim), shots=1).result()
    counts = result.get_counts()
    outcome = list(counts.keys())[0]

    return 'constant' if outcome == '0' * n else 'balanced'


# Example: constant oracle (f(x) = 0 always)
n = 3
constant_oracle = QuantumCircuit(n + 1)  # does nothing

# Example: balanced oracle (f(x) = x_0, flips ancilla when qubit 0 is |1⟩)
balanced_oracle = QuantumCircuit(n + 1)
balanced_oracle.cx(0, n)

print(deutsch_jozsa(constant_oracle, n))   # 'constant'
print(deutsch_jozsa(balanced_oracle, n))   # 'balanced'
```

---

## Bernstein-Vazirani

See theory: [[../Algorithms/Bernstein-Vazirani Algorithm]]

```python
from qiskit import QuantumCircuit
from qiskit_aer import AerSimulator
from qiskit import transpile

def bernstein_vazirani(s: str) -> str:
    """Find hidden bitstring s using 1 query."""
    n = len(s)
    qc = QuantumCircuit(n + 1, n)

    # Ancilla in |−⟩
    qc.x(n)
    qc.h(range(n + 1))

    # Oracle: f(x) = s·x mod 2
    # For each bit s_i = 1, apply CNOT from qubit i to ancilla
    for i, bit in enumerate(reversed(s)):
        if bit == '1':
            qc.cx(i, n)

    qc.h(range(n))
    qc.measure(range(n), range(n))

    sim = AerSimulator()
    result = sim.run(transpile(qc, sim), shots=1).result()
    return list(result.get_counts().keys())[0][::-1]  # reverse bit order

print(bernstein_vazirani('10110'))  # '10110'
```

---

## Grover's Algorithm

See theory: [[../Algorithms/Grover's Algorithm]]

```python
from qiskit import QuantumCircuit
from qiskit.circuit.library import GroverOperator, PhaseOracle
from qiskit_aer import AerSimulator
from qiskit import transpile
import numpy as np

def grover(oracle_expression: str, n_qubits: int) -> dict:
    """
    oracle_expression: boolean expression, e.g. '(x0 & x1 & ~x2)'
    Returns measurement counts after optimal Grover iterations.
    """
    oracle = PhaseOracle(oracle_expression)
    grover_op = GroverOperator(oracle)

    # Optimal number of iterations
    n_solutions = 1  # assume 1 marked item
    n_iter = int(np.round(np.pi / 4 * np.sqrt(2**n_qubits / n_solutions)))

    qc = QuantumCircuit(n_qubits, n_qubits)
    qc.h(range(n_qubits))                          # uniform superposition
    for _ in range(n_iter):
        qc.compose(grover_op, inplace=True)         # oracle + diffusion
    qc.measure_all()

    sim = AerSimulator()
    result = sim.run(transpile(qc, sim), shots=1024).result()
    return result.get_counts()

# Find x such that x0=1, x1=1, x2=0
counts = grover('(x0 & x1 & ~x2)', 3)
print(sorted(counts.items(), key=lambda x: -x[1])[:3])
```

---

## Quantum Fourier Transform

See theory: [[../Algorithms/Quantum Fourier Transform]]

```python
from qiskit.circuit.library import QFT
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

# Built-in QFT
n = 4
qft = QFT(n)
print(qft.decompose().draw('text'))

# Apply QFT to a specific state
qc = QuantumCircuit(n)
qc.x(0)          # start in |0001⟩ = |1⟩
qc.compose(QFT(n), inplace=True)

sv = Statevector(qc)
print(sv.probabilities_dict())
# Uniform distribution — QFT of a basis state is a uniform superposition with phases
```

---

## Quantum Phase Estimation

See theory: [[../Algorithms/Quantum Phase Estimation]]

```python
from qiskit import QuantumCircuit
from qiskit.circuit.library import QFT
from qiskit_aer import AerSimulator
from qiskit import transpile

def qpe(unitary: QuantumCircuit, eigenstate: QuantumCircuit, n_ancilla: int) -> float:
    """Estimate the phase φ of U|u⟩ = e^{2πiφ}|u⟩."""
    n_target = eigenstate.num_qubits
    qc = QuantumCircuit(n_ancilla + n_target, n_ancilla)

    # Prepare eigenstate
    qc.compose(eigenstate, qubits=range(n_ancilla, n_ancilla + n_target), inplace=True)

    # Hadamard on ancilla
    qc.h(range(n_ancilla))

    # Controlled-U^(2^k)
    for k in range(n_ancilla):
        controlled_u = unitary.power(2**k).control(1)
        qc.compose(controlled_u,
                   qubits=[k] + list(range(n_ancilla, n_ancilla + n_target)),
                   inplace=True)

    # Inverse QFT
    qc.compose(QFT(n_ancilla, inverse=True), qubits=range(n_ancilla), inplace=True)
    qc.measure(range(n_ancilla), range(n_ancilla))

    sim = AerSimulator()
    result = sim.run(transpile(qc, sim), shots=1024).result()
    counts = result.get_counts()
    most_likely = max(counts, key=counts.get)
    phase_int = int(most_likely, 2)
    return phase_int / 2**n_ancilla

# Example: T gate has eigenvalue e^{iπ/4}, so phase = 1/8
T_gate = QuantumCircuit(1)
T_gate.t(0)

eigenstate = QuantumCircuit(1)
eigenstate.x(0)   # |1⟩ is eigenstate of T with eigenvalue e^{iπ/4}

phase = qpe(T_gate, eigenstate, n_ancilla=4)
print(f"Estimated phase: {phase:.4f}")   # ≈ 0.125 = 1/8
print(f"Actual phase: {1/8:.4f}")
```

---

## VQE — Variational Quantum Eigensolver

See theory: [[../Algorithms/Variational Quantum Eigensolver]]

```python
from qiskit.circuit.library import TwoLocal
from qiskit.quantum_info import SparsePauliOp
from qiskit_aer import AerSimulator
from qiskit import transpile
from scipy.optimize import minimize
import numpy as np

# H2 molecule Hamiltonian (simplified, 2-qubit)
H = SparsePauliOp.from_list([
    ('II', -1.0523732),
    ('IZ',  0.3979374),
    ('ZI', -0.3979374),
    ('ZZ', -0.0112801),
    ('XX',  0.1809312)
])

# Ansatz: parameterised circuit
ansatz = TwoLocal(2, ['ry', 'rz'], 'cx', reps=2)
n_params = ansatz.num_parameters

def energy(params):
    """Evaluate ⟨ψ(θ)|H|ψ(θ)⟩."""
    bound = ansatz.assign_parameters(params)
    from qiskit.quantum_info import Statevector
    sv = Statevector(bound)
    return sv.expectation_value(H).real

# Optimise
result = minimize(energy, x0=np.random.uniform(0, 2*np.pi, n_params),
                  method='COBYLA', options={'maxiter': 500})

print(f"Ground state energy: {result.fun:.6f} Ha")
print(f"Exact (FCI):        -1.137270 Ha")
```

---

## QAOA — Max-Cut

See theory: [[../Algorithms/Quantum Approximate Optimization Algorithm]]

```python
from qiskit.circuit.library import QAOAAnsatz
from qiskit.quantum_info import SparsePauliOp, Statevector
from scipy.optimize import minimize
import numpy as np

# Max-Cut on a triangle graph: edges (0,1), (1,2), (0,2)
# H_C = (1/2)[(I-Z0Z1) + (I-Z1Z2) + (I-Z0Z2)]
H_C = SparsePauliOp.from_list([
    ('IZZ', -0.5), ('ZIZ', -0.5), ('ZZI', -0.5),
    ('III',  1.5)
])

p = 2  # QAOA depth
ansatz = QAOAAnsatz(H_C, reps=p)
n_params = ansatz.num_parameters

def cost(params):
    bound = ansatz.assign_parameters(params)
    sv = Statevector(bound)
    return -sv.expectation_value(H_C).real  # negative because we maximise

result = minimize(cost, x0=np.random.uniform(0, np.pi, n_params), method='COBYLA')

# Sample the optimised state
bound = ansatz.assign_parameters(result.x)
sv = Statevector(bound)
probs = sv.probabilities_dict()
best = max(probs, key=probs.get)
print(f"Best cut: {best}, probability: {probs[best]:.3f}")
# For a triangle, max cut = 2 edges. Solutions: 001, 010, 100, 110, 101, 011
```

---

> Up: [[Qiskit MOC]]
