# Running on Real Hardware

> [[Qiskit MOC]] | Prev: [[Qiskit Simulators]] | Next: [[Qiskit — Implementing the Algorithms]]

---

## The Pipeline: From Circuit to Hardware

```
Your QuantumCircuit
        ↓
Transpilation (compile to native gates + qubit mapping)
        ↓
IBM Quantum Runtime (job queue)
        ↓
Real superconducting qubits
        ↓
Measurement results (counts)
        ↓
Error mitigation (optional post-processing)
        ↓
Your answer
```

Each step matters. Understanding it prevents surprises.

---

## Step 1: Connect to IBM Quantum

```python
from qiskit_ibm_runtime import QiskitRuntimeService

# First time: save your API token
QiskitRuntimeService.save_account(
    channel='ibm_quantum',
    token='YOUR_IBM_QUANTUM_API_TOKEN',
    overwrite=True
)

# Every subsequent time
service = QiskitRuntimeService()

# List available backends
for backend in service.backends():
    print(backend.name, backend.num_qubits, backend.status().pending_jobs)
```

Get your free API token at: https://quantum.ibm.com

---

## Step 2: Choose a Backend

```python
# Get a specific backend
backend = service.backend('ibm_brisbane')  # 127-qubit Eagle processor

# Or let Qiskit pick the least busy one
backend = service.least_busy(operational=True, simulator=False)

# Inspect the backend
print(backend.num_qubits)
print(backend.coupling_map)   # which qubits are physically connected
print(backend.operation_names) # native gate set
```

**Key properties to check:**
- `num_qubits` — how many qubits
- `coupling_map` — which pairs of qubits can have a CNOT (not all pairs are connected)
- `basis_gates` — the native gate set (e.g., `['cx', 'id', 'rz', 'sx', 'x']`)
- Error rates per gate (from `backend.properties()`)

---

## Step 3: Transpilation

Your circuit uses abstract gates (H, CNOT, T...) on abstract qubits (0, 1, 2...). Real hardware has:
- A specific native gate set (e.g., only `{CX, RZ, SX, X}`)
- A specific connectivity (not all qubit pairs are connected)

**Transpilation** compiles your circuit to the hardware's constraints:

```python
from qiskit import transpile

# Transpile for the target backend
transpiled_qc = transpile(
    qc,
    backend=backend,
    optimization_level=3   # 0=none, 1=light, 2=medium, 3=heavy
)

print(f"Original depth: {qc.depth()}")
print(f"Transpiled depth: {transpiled_qc.depth()}")
print(f"Gate counts: {transpiled_qc.count_ops()}")
```

`optimization_level=3` applies the most aggressive circuit optimisation — always use this for real hardware. It can dramatically reduce gate count and depth.

**What transpilation does:**
1. **Routing:** Inserts SWAP gates to move qubits next to each other when a CNOT is needed between non-adjacent qubits
2. **Basis translation:** Decomposes your gates into the native gate set
3. **Optimisation:** Cancels redundant gates, merges rotations

---

## Step 4: Run with Qiskit Runtime

The modern way to run on IBM hardware uses **Primitives** — higher-level abstractions that handle shots, error mitigation, and batching.

```python
from qiskit_ibm_runtime import SamplerV2 as Sampler

# Sampler: runs circuits and returns measurement counts
sampler = Sampler(backend)

job = sampler.run([transpiled_qc], shots=1024)
result = job.result()

counts = result[0].data.meas.get_counts()
print(counts)
```

For expectation values (VQE/QAOA):

```python
from qiskit_ibm_runtime import EstimatorV2 as Estimator
from qiskit.quantum_info import SparsePauliOp

estimator = Estimator(backend)

observable = SparsePauliOp('ZZ')
job = estimator.run([(transpiled_qc, observable)])
result = job.result()

expectation_value = result[0].data.evs
print(expectation_value)
```

---

## Step 5: Error Mitigation

Real hardware gives noisy results. Error mitigation post-processes the results to reduce the effect of noise — without the overhead of full error correction.

```python
from qiskit_ibm_runtime import EstimatorV2 as Estimator
from qiskit_ibm_runtime.options import EstimatorOptions

options = EstimatorOptions()
options.resilience_level = 1   # 0=none, 1=basic, 2=ZNE, 3=PEC

estimator = Estimator(backend, options=options)
```

**Resilience levels:**
- `0`: No mitigation — raw noisy results
- `1`: Readout error mitigation — corrects measurement errors
- `2`: Zero-Noise Extrapolation (ZNE) — runs at multiple noise levels and extrapolates to zero noise
- `3`: Probabilistic Error Cancellation (PEC) — most accurate, most expensive

---

## Understanding Job Status

```python
job = sampler.run([qc], shots=1024)

print(job.job_id())          # save this to retrieve results later
print(job.status())          # QUEUED, RUNNING, DONE, ERROR

# Retrieve a previous job
old_job = service.job('JOB_ID_HERE')
result = old_job.result()
```

IBM quantum computers are shared resources. Queue times range from seconds to hours depending on the backend and time of day. Free accounts have limited monthly compute time.

---

## The Coupling Map: Why It Matters

Real qubits are not all connected. IBM's Eagle processor (127 qubits) has a heavy-hex connectivity — each qubit connects to at most 3 others.

If your circuit needs a CNOT between qubits 0 and 10 (not adjacent), transpilation inserts SWAP gates to route qubit 0 next to qubit 10. Each SWAP = 3 CNOTs = 3× the error. Deep circuits with many non-local gates get very noisy.

**Best practice:** Design circuits that respect the coupling map, or use `optimization_level=3` and let Qiskit find the best qubit mapping.

```python
# See the coupling map
from qiskit.visualization import plot_coupling_map
plot_coupling_map(backend.num_qubits, None, backend.coupling_map.get_edges())
```

---

## NISQ Reality Check

Current IBM hardware (2024-2026):
- ~100–1000 physical qubits
- Gate error rates: ~0.1% (single-qubit), ~0.5% (two-qubit)
- $T_1, T_2 \sim 100\ \mu s$
- Circuit depth limit before decoherence: ~100–1000 gates

This means:
- Shor's algorithm for RSA: **not possible** (needs millions of qubits with error correction)
- Grover's on 20 qubits: **marginal** (circuit too deep)
- VQE for small molecules (H₂, LiH): **possible** with error mitigation
- QAOA for small graphs: **possible**
- Quantum simulation of small systems: **possible**

---

> Up: [[Qiskit MOC]] | Next: [[Qiskit — Implementing the Algorithms]]
