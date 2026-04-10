# Quantum Computing — Map of Content

Quantum computing is a fundamentally different model of computation — one that exploits the physics of quantum mechanics to solve certain problems exponentially faster than any classical computer. The math is linear algebra and probability you already know, but applied to a strange new world where vectors can cancel each other out.

> **The core idea:** A quantum computer doesn't try all answers at once. It uses *interference* — amplifying paths that lead to the right answer and cancelling paths that don't. By the time you measure, the right answer has probability close to 1.

---

## 🧱 Foundations

Work through these in order. Each one builds on the last.

| Note | What you'll understand after |
|---|---|
| [[Foundations/Qubits and Superposition]] | What a qubit actually is, what superposition means physically and mathematically, why $n$ qubits is powerful |
| [[Foundations/Quantum States and Dirac Notation]] | The $\|\psi\rangle$ notation, inner products, tensor products, why complex numbers are necessary |
| [[Foundations/Quantum Gates]] | Unitary matrices, Hadamard, Pauli gates, CNOT — the building blocks of every circuit |
| [[Foundations/Measurement and Collapse]] | The Born rule, why measurement is destructive, projective measurement, partial measurement |
| [[Foundations/Entanglement]] | What entanglement is (and isn't), Bell states, why it's a computational resource |
| [[Foundations/Quantum Circuits]] | How to read and build circuits, oracle gates, circuit depth and why it matters |
| [[Foundations/Interference]] | The actual mechanism behind quantum speedup — constructive and destructive interference |
| [[Foundations/Phase Kickback]] | The trick every quantum algorithm uses to encode $f(x)$ into phases |
| [[Foundations/Density Matrices and Mixed States]] | Mixed states, decoherence in math, Von Neumann entropy, how noise is modelled |
| [[Foundations/Quantum Noise and Decoherence]] | Why real qubits fail, $T_1$/$T_2$ times, error types, the threshold theorem |
| [[Foundations/Quantum Error Correction]] | No-cloning theorem, stabiliser codes, the surface code, the cost of fault tolerance |

---

## ⚙️ Algorithms

| Algorithm | Problem | Speedup | Key technique |
|---|---|---|---|
| [[Algorithms/Deutsch-Jozsa Algorithm]] | Constant vs balanced function? | Exponential | Phase kickback + interference |
| [[Algorithms/Bernstein-Vazirani Algorithm]] | Find hidden bitstring $s$ | Exponential | Fourier transform over $\mathbb{Z}_2^n$ |
| [[Algorithms/Simon's Algorithm]] | Find hidden XOR period | Exponential | Periodic state + linear algebra |
| [[Algorithms/Grover's Algorithm]] | Search unsorted database | Quadratic ($\sqrt{N}$) | Amplitude amplification |
| [[Algorithms/Quantum Fourier Transform]] | Fourier transform on amplitudes | Exponential | Product form, $O(n^2)$ gates |
| [[Algorithms/Quantum Phase Estimation]] | Find eigenphase of a unitary | Exponential | QFT + phase kickback |
| [[Algorithms/Shor's Algorithm]] | Factor large integers | Exponential | Period finding via QPE + QFT |
| [[Algorithms/HHL Algorithm]] | Solve linear system $Ax=b$ | Exponential* | QPE + eigenvalue inversion |
| [[Algorithms/Quantum Simulation]] | Simulate quantum systems | Exponential | Trotterisation + QPE |
| [[Algorithms/Variational Quantum Eigensolver]] | Ground state energy | Heuristic | Variational principle + gradient descent |
| [[Algorithms/Quantum Approximate Optimization Algorithm]] | Combinatorial optimisation | Heuristic | Trotterisation + adiabatic inspiration |

*HHL has caveats — see the note.

---

## 📡 Protocols

| Protocol | What it does |
|---|---|
| [[Protocols/Quantum Teleportation]] | Transfer a qubit state using 1 ebit + 2 classical bits |
| [[Protocols/Superdense Coding]] | Send 2 classical bits using 1 qubit + 1 ebit |
| [[Protocols/BB84 Quantum Key Distribution]] | Unconditionally secure key exchange using quantum mechanics |

---

## 💻 Qiskit

| Note | What you'll be able to do |
|---|---|
| [[Qiskit/Qiskit Basics — Circuits and Gates]] | Build any circuit, apply any gate, inspect state vectors |
| [[Qiskit/Qiskit Simulators]] | Statevector, shot-based, noise simulation, expectation values |
| [[Qiskit/Running on Real Hardware]] | Transpilation, IBM Runtime, error mitigation, NISQ constraints |
| [[Qiskit/Qiskit — Implementing the Algorithms]] | Full working code for every algorithm in this vault |

---

## 🌍 Context

| Note | What it covers |
|---|---|
| [[Quantum Complexity Theory]] | BQP, NP, QMA — when does a quantum computer actually help? |
| [[The NISQ Era]] | Where we actually are, what's possible now vs future, honest assessment |

---

## 🔗 Bridge to ML and DL

> Full bridge note: [[Quantum Computing and ML — Connections]]

| QC concept | ML equivalent |
|---|---|
| Qubit state = unit vector in $\mathbb{C}^{2^n}$ | [[../Mathematics of ML and DL/Foundations/Linear Algebra/Vectors and Vector Spaces]] |
| Quantum gates = unitary matrices | [[../Mathematics of ML and DL/Foundations/Linear Algebra/Matrices and Operations]] |
| Measurement = projection + Born rule | [[../Mathematics of ML and DL/Foundations/Linear Algebra/Eigenvalues and Eigenvectors]] |
| QFT = quantum eigendecomposition | [[../Mathematics of ML and DL/Foundations/Linear Algebra/SVD and PCA]] |
| Born rule = probability from amplitudes | [[../Mathematics of ML and DL/Foundations/Statistics and Probability/Probability Basics]] |
| VQE/QAOA = gradient descent on circuits | [[../Mathematics of ML and DL/Foundations/Calculus/Gradient Descent]] |
| Parameter shift rule = quantum gradient | [[../Mathematics of ML and DL/Foundations/Calculus/Partial Derivatives and Gradients]] |
| Barren plateaus = vanishing gradients | [[../Mathematics of ML and DL/DL Models/Recurrent Neural Network]] |
| Quantum kernels = kernel trick | [[../Mathematics of ML and DL/ML Models/Support Vector Machines]] |

---

## 🗺️ Suggested Learning Path

**Complete beginner — foundations:**
[[Foundations/Qubits and Superposition]] → [[Foundations/Quantum States and Dirac Notation]] → [[Foundations/Quantum Gates]] → [[Foundations/Measurement and Collapse]] → [[Foundations/Entanglement]] → [[Foundations/Quantum Circuits]] → [[Foundations/Interference]] → [[Foundations/Phase Kickback]]

**Algorithms (in this order — each builds on the previous):**
[[Algorithms/Deutsch-Jozsa Algorithm]] → [[Algorithms/Bernstein-Vazirani Algorithm]] → [[Algorithms/Simon's Algorithm]] → [[Algorithms/Grover's Algorithm]] → [[Algorithms/Quantum Fourier Transform]] → [[Algorithms/Quantum Phase Estimation]] → [[Algorithms/Shor's Algorithm]] → [[Algorithms/HHL Algorithm]] → [[Algorithms/Quantum Simulation]]

**Protocols (can be done any time after Entanglement):**
[[Protocols/Quantum Teleportation]] → [[Protocols/Superdense Coding]] → [[Protocols/BB84 Quantum Key Distribution]]

**Noise and hardware:**
[[Foundations/Density Matrices and Mixed States]] → [[Foundations/Quantum Noise and Decoherence]] → [[Foundations/Quantum Error Correction]] → [[The NISQ Era]]

**Variational algorithms (after Grover + noise):**
[[Algorithms/Variational Quantum Eigensolver]] → [[Algorithms/Quantum Approximate Optimization Algorithm]]

**Qiskit (run alongside algorithms):**
[[Qiskit/Qiskit Basics — Circuits and Gates]] → [[Qiskit/Qiskit Simulators]] → [[Qiskit/Qiskit — Implementing the Algorithms]] → [[Qiskit/Running on Real Hardware]]

**Context:**
[[Quantum Complexity Theory]] → [[The NISQ Era]] → [[Quantum Computing and ML — Connections]]
