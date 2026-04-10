# Qiskit — Map of Content

Qiskit is IBM's open-source Python framework for quantum computing. It lets you build quantum circuits, simulate them classically, and run them on real IBM quantum hardware.

> **Mental model:** Qiskit is to quantum computing what PyTorch is to deep learning — a framework that handles the low-level hardware details so you can focus on the algorithm.

---

## 🗂️ Notes in This Section

| Note | What you'll be able to do |
|---|---|
| [[Qiskit Basics — Circuits and Gates]] | Build any quantum circuit, apply any gate, measure qubits |
| [[Qiskit Simulators]] | Run circuits on statevector, density matrix, and noise simulators |
| [[Running on Real Hardware]] | Submit jobs to IBM quantum computers, understand transpilation |
| [[Qiskit — Implementing the Algorithms]] | Implement every algorithm in this vault in actual Qiskit code |

---

## The Qiskit Stack

```
Your Python code (algorithms, circuits)
        ↓
Qiskit Terra — core circuit building and compilation
        ↓
Qiskit Aer — classical simulators (statevector, noise)
        ↓
Qiskit IBM Runtime — interface to real IBM hardware
        ↓
IBM Quantum hardware (superconducting qubits)
```

---

> Start here: [[Qiskit Basics — Circuits and Gates]]
