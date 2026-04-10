# Quantum Simulation

> [[../QC MOC]] | Prereqs: [[Quantum Phase Estimation]], [[../Foundations/Quantum Gates]]

---

## The Original Motivation for Quantum Computing

In 1981, Richard Feynman asked: "Can we simulate physics on a computer?" His answer: classical computers can't efficiently simulate quantum systems — the state space grows exponentially. But a quantum computer could simulate quantum systems efficiently, because it *is* a quantum system.

Quantum simulation is arguably the most near-term useful application of quantum computers — more practical than Shor's algorithm (which requires fault tolerance) and more principled than VQE/QAOA (which are heuristics).

---

## The Problem

Given a Hamiltonian $H$ describing a physical system, simulate its time evolution:

$$|\psi(t)\rangle = e^{-iHt}|\psi(0)\rangle$$

For a system of $n$ particles, $H$ is a $2^n \times 2^n$ matrix. Classically, computing $e^{-iHt}$ requires $O(2^{3n})$ operations — exponential. A quantum computer can do it in $O(\text{poly}(n, t))$.

---

## Trotterisation: Breaking Up the Exponential

Most Hamiltonians are sums of simpler terms: $H = H_1 + H_2 + \cdots + H_k$

The problem: $e^{-i(H_1 + H_2)t} \neq e^{-iH_1 t}e^{-iH_2 t}$ in general (because $H_1$ and $H_2$ may not commute).

**First-order Trotter formula:**

$$e^{-i(H_1+H_2)t} \approx \left(e^{-iH_1 t/r}e^{-iH_2 t/r}\right)^r$$

The error is $O(t^2/r)$ — decreasing the step size $t/r$ improves accuracy. This is the quantum analogue of Euler's method for ODEs.

**Second-order (Suzuki-Trotter):**

$$e^{-i(H_1+H_2)t} \approx \left(e^{-iH_1 t/2r}e^{-iH_2 t/r}e^{-iH_1 t/2r}\right)^r$$

Error $O(t^3/r^2)$ — much better. Higher-order formulas exist.

---

## Simulating a Simple Hamiltonian in Qiskit

**Example:** Two-qubit Ising model $H = J Z_0 Z_1 + h(X_0 + X_1)$

The time evolution $e^{-iHt}$ is implemented as:

```python
from qiskit import QuantumCircuit
import numpy as np

def ising_trotter_step(J, h, dt):
    """One Trotter step for H = J*ZZ + h*(X+X)."""
    qc = QuantumCircuit(2)
    
    # e^{-i J dt Z0 Z1}: ZZ interaction
    qc.cx(0, 1)
    qc.rz(2 * J * dt, 1)
    qc.cx(0, 1)
    
    # e^{-i h dt X0} and e^{-i h dt X1}: transverse field
    qc.rx(2 * h * dt, 0)
    qc.rx(2 * h * dt, 1)
    
    return qc

def ising_simulation(J, h, total_time, n_steps):
    dt = total_time / n_steps
    qc = QuantumCircuit(2)
    for _ in range(n_steps):
        qc.compose(ising_trotter_step(J, h, dt), inplace=True)
    return qc
```

---

## Implementing $e^{-i\theta P}$ for Pauli Strings

Any Pauli string $P = P_1 \otimes P_2 \otimes \cdots \otimes P_n$ (where each $P_i \in \{I, X, Y, Z\}$) can be exponentiated as a quantum circuit:

1. **Diagonalise:** Apply basis-change gates to rotate each non-identity Pauli to $Z$
   - $X \to Z$: apply $H$
   - $Y \to Z$: apply $S^\dagger H$
   - $Z \to Z$: do nothing
2. **Compute parity:** Apply a ladder of CNOTs to compute the parity of all $Z$ qubits into one target qubit
3. **Rotate:** Apply $R_z(2\theta)$ to the target qubit
4. **Uncompute:** Reverse the CNOT ladder and basis changes

This circuit implements $e^{-i\theta P}$ exactly.

---

## Quantum Phase Estimation for Energy Levels

To find the energy eigenvalues of $H$, use [[Quantum Phase Estimation]] with $U = e^{-iHt}$:

- Eigenvalues of $U$ are $e^{-iE_k t}$
- QPE measures the phase $\varphi_k = E_k t / 2\pi$
- Dividing by $t$ gives $E_k$

This is the quantum simulation approach to quantum chemistry — more systematic than VQE, but requires deeper circuits (fault tolerance needed).

---

## Applications

| Application | System simulated | Near-term? |
|---|---|---|
| Quantum chemistry | Molecular Hamiltonians (drug discovery, materials) | VQE now; QPE later |
| Condensed matter | Hubbard model, spin chains, topological phases | Near-term |
| High-energy physics | Lattice gauge theories, particle interactions | Medium-term |
| Optimisation | Adiabatic simulation, quantum annealing | Near-term |
| Finance | Stochastic differential equations | Near-term (heuristic) |

---

> Up: [[../QC MOC]] | See also: [[../Algorithms/Variational Quantum Eigensolver]], [[Quantum Phase Estimation]]
