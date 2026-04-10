# ⚛ Quantum Computing — From First Principles to Qiskit

A complete, self-contained knowledge vault for mastering Quantum Computing — built as interconnected Markdown notes with deep mathematical explanations, designed for someone coming from a Machine Learning background.

> **[🔭 Open Interactive Graph View](https://YOUR_USERNAME.github.io/YOUR_REPO/)** — explore all 34 notes and their connections visually

---

## What's Inside

| Section | Notes | Coverage |
|---|---|---|
| **Foundations** | 11 | Qubits, gates, measurement, entanglement, interference, phase kickback, noise, error correction |
| **Algorithms** | 11 | Deutsch-Jozsa → Bernstein-Vazirani → Simon's → Grover → QFT → QPE → Shor → HHL → Simulation → VQE → QAOA |
| **Protocols** | 3 | Quantum teleportation, superdense coding, BB84 QKD |
| **Qiskit** | 4 | Circuits & gates, simulators, real hardware, full algorithm implementations |
| **Context** | 4 | Complexity theory (BQP/NP/QMA), NISQ era, ML connections, MOC |

---

## Graph Preview

<!-- Static SVG fallback for GitHub rendering -->
```
QC MOC (hub)
├── Foundations
│   ├── Qubits and Superposition
│   │   └── Quantum States and Dirac Notation
│   │       └── Quantum Gates
│   │           └── Measurement and Collapse
│   │               └── Entanglement
│   │                   ├── Quantum Circuits
│   │                   │   └── Interference
│   │                   │       └── Phase Kickback
│   │                   └── Quantum Error Correction
│   ├── Density Matrices and Mixed States
│   └── Quantum Noise and Decoherence
│
├── Algorithms
│   ├── Deutsch-Jozsa → Bernstein-Vazirani → Simon's
│   ├── Grover's Algorithm
│   ├── Quantum Fourier Transform
│   │   └── Quantum Phase Estimation
│   │       └── Shor's Algorithm
│   ├── HHL Algorithm
│   ├── Quantum Simulation
│   └── VQE → QAOA
│
├── Protocols
│   └── Teleportation → Superdense Coding → BB84
│
└── Qiskit
    ├── Basics — Circuits and Gates
    ├── Simulators
    ├── Running on Real Hardware
    └── Implementing the Algorithms
```

---

## How to Use

**In Obsidian:** Clone/download and open the folder as a vault. Use Graph View (`Ctrl+G`) for the full interactive experience.

**On GitHub:** Browse notes directly, or open the [interactive graph](https://YOUR_USERNAME.github.io/YOUR_REPO/) (enable GitHub Pages → source: root → `index.html`).

**To enable the graph on GitHub Pages:**
1. Go to repo Settings → Pages
2. Source: Deploy from branch → `main` → `/ (root)`
3. Rename `graph.html` to `index.html` (or set it as the entry point)
4. In `graph.html`, replace `YOUR_USERNAME/YOUR_REPO` with your actual GitHub username and repo name
5. Visit `https://YOUR_USERNAME.github.io/YOUR_REPO/`

---

## Learning Path

**Beginner → Algorithms → Qiskit:**

`Qubits and Superposition` → `Quantum States and Dirac Notation` → `Quantum Gates` → `Measurement and Collapse` → `Entanglement` → `Quantum Circuits` → `Interference` → `Phase Kickback` → `Deutsch-Jozsa` → `Grover's` → `QFT` → `Shor's` → `VQE` → `Qiskit Basics` → `Implementing the Algorithms`

---

## Philosophy

Every note follows the same structure:
1. **The problem / intuition** — why does this exist, what does it solve
2. **The math** — every symbol explained, every step shown
3. **Worked examples** — concrete numbers, not just formulas
4. **Connections** — links to related notes and ML equivalents

The math is not dumbed down. The explanations make it accessible.

---

## Connection to ML/DL

This vault connects directly to the Mathematics of ML and DL:

| QC Concept | ML Equivalent |
|---|---|
| Quantum state = unit vector in $\mathbb{C}^{2^n}$ | Weight vectors, embeddings |
| Quantum gates = unitary matrices | Linear layers (constrained) |
| QFT = quantum eigendecomposition | SVD / PCA |
| VQE/QAOA = gradient descent on circuits | Neural network training |
| Barren plateaus | Vanishing gradients in deep RNNs |
| Quantum kernels | Kernel trick in SVMs |
