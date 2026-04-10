<div align="center">

# ⚛ Quantum Computing
### From First Principles to Qiskit

<br/>

[![Graph View](https://img.shields.io/badge/🔭_Interactive_Graph-Open_Now-58a6ff?style=for-the-badge&logoColor=white)](https://anuraagrath.github.io/QuantumComputingCourse/)
[![Notes](https://img.shields.io/badge/Notes-34-3fb950?style=for-the-badge)](.)
[![Connections](https://img.shields.io/badge/Connections-97-d2a8ff?style=for-the-badge)](.)
[![Built With](https://img.shields.io/badge/Built_with-Obsidian_+_D3.js-e3b341?style=for-the-badge)](.)

<br/>

> *A complete knowledge vault for mastering Quantum Computing — built as interconnected Markdown notes with deep mathematical explanations, designed for someone coming from a Machine Learning / Deep Learning background.*

</div>

---

## 🗺️ Interactive Graph View

<div align="center">

**Click any node → opens the note. Hover → highlights connections. Search → filters the graph.**

[![Open Graph](https://img.shields.io/badge/Open_Interactive_Graph_View_%E2%86%92-0d1117?style=for-the-badge&logo=github&logoColor=58a6ff&labelColor=161b22)](https://anuraagrath.github.io/QuantumComputingCourse/)

</div>

---

## 📚 What's Inside

<table>
<tr>
<td width="50%">

### 🔵 Foundations
| Note | Core Idea |
|---|---|
| Qubits and Superposition | What a qubit actually *is* |
| Quantum States & Dirac Notation | The math language of QC |
| Quantum Gates | Unitary matrices as operations |
| Measurement and Collapse | The Born rule, irreversibility |
| Entanglement | Inseparable correlations |
| Quantum Circuits | Building quantum programs |
| Interference | The real source of speedup |
| Phase Kickback | The trick every algorithm uses |
| Density Matrices | Math of noise and decoherence |
| Quantum Noise & Decoherence | Why real qubits fail |
| Quantum Error Correction | Stabiliser codes, surface code |

</td>
<td width="50%">

### 🟢 Algorithms
| Algorithm | Speedup |
|---|---|
| Deutsch-Jozsa | Exponential |
| Bernstein-Vazirani | Exponential |
| Simon's Algorithm | Exponential |
| Grover's Search | Quadratic $O(\sqrt{N})$ |
| Quantum Fourier Transform | Exponential |
| Quantum Phase Estimation | Exponential |
| Shor's Factoring | Exponential 💥 |
| HHL Linear Systems | Exponential* |
| Quantum Simulation | Exponential |
| VQE | Heuristic |
| QAOA | Heuristic |

</td>
</tr>
<tr>
<td>

### 🟡 Protocols
| Protocol | What it does |
|---|---|
| Quantum Teleportation | Transfer qubit state using entanglement |
| Superdense Coding | 2 classical bits via 1 qubit |
| BB84 QKD | Unbreakable key exchange |

</td>
<td>

### 🟣 Qiskit
| Note | What you can do after |
|---|---|
| Basics — Circuits & Gates | Build any circuit |
| Simulators | Statevector, noise, shots |
| Running on Real Hardware | IBM Runtime, transpilation |
| Implementing the Algorithms | Full working code for everything |

</td>
</tr>
</table>

---

## 🧭 Learning Path

```
START HERE
    │
    ▼
🔵 Qubits & Superposition
    │
    ▼
🔵 Quantum States & Dirac Notation ──► 🔵 Quantum Gates
                                              │
                                              ▼
                                    🔵 Measurement & Collapse
                                              │
                              ┌───────────────┼───────────────┐
                              ▼               ▼               ▼
                         🔵 Entanglement  🔵 Interference  🔵 Noise
                              │               │
                              ▼               ▼
                    🟡 Teleportation    🔵 Phase Kickback
                                              │
                              ┌───────────────┼───────────────┐
                              ▼               ▼               ▼
                        🟢 Deutsch-Jozsa  🟢 Grover's    🟢 QFT
                              │                               │
                              ▼                               ▼
                        🟢 BV / Simon's              🟢 Phase Estimation
                                                             │
                                                    ┌────────┴────────┐
                                                    ▼                 ▼
                                              🟢 Shor's           🟢 HHL
                                                    │
                                                    ▼
                                              🟢 VQE / QAOA
                                                    │
                                                    ▼
                                            🟣 Qiskit — Build It
```

---

## 🔗 Connection to ML / DL

| Quantum Computing | Machine Learning Equivalent |
|---|---|
| Qubit state $\|\psi\rangle$ = unit vector in $\mathbb{C}^{2^n}$ | Weight vectors, embeddings |
| Quantum gates = unitary matrices | Linear layers (reversibility constrained) |
| Quantum Fourier Transform | SVD / PCA — finding the natural basis |
| Born rule = $P(x) = \|\langle x\|\psi\rangle\|^2$ | Softmax probability from logits |
| VQE / QAOA training loop | Gradient descent on neural networks |
| Parameter shift rule | Backpropagation |
| Barren plateaus | Vanishing gradients in deep RNNs |
| Quantum kernel methods | Kernel trick in SVMs |

---

## 🧠 Philosophy

Every note follows the same structure:

1. **The problem / intuition** — why does this exist, what does it solve
2. **The math** — every symbol explained, every step shown, nothing skipped
3. **Worked examples** — concrete numbers, not just abstract formulas
4. **Connections** — wikilinks to related notes and ML equivalents

> The math is not dumbed down. The explanations make it accessible.

---

<div align="center">

Made with 🖤 by Anuraag Rath

</div>
