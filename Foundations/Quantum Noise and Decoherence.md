# Quantum Noise and Decoherence

> [[../QC MOC]] | Prev: [[Phase Kickback]] | Next: [[Quantum Error Correction]]

---

## The Fundamental Engineering Problem

Everything we've discussed so far assumes perfect qubits — states that stay exactly as you set them, gates that work exactly as specified. Real quantum hardware is nothing like this.

Real qubits are physical objects — superconducting circuits, trapped ions, photons, nitrogen-vacancy centres in diamond. They interact with their environment. And any interaction with the environment **destroys quantum information**.

This is the central engineering challenge of quantum computing. It's why we don't have a 1000-qubit fault-tolerant quantum computer yet, even though the theory has been known for decades.

---

## What is Decoherence?

A qubit in superposition $\alpha|0\rangle + \beta|1\rangle$ has a definite phase relationship between $|0\rangle$ and $|1\rangle$. This phase is what enables interference.

**Decoherence** is the process by which the environment "measures" the qubit without you knowing. The qubit becomes entangled with the environment, and the phase relationship is lost. The qubit behaves like a classical probabilistic mixture rather than a quantum superposition.

Mathematically: a pure state $|\psi\rangle\langle\psi|$ becomes a **mixed state** — a statistical mixture described by a density matrix:

$$\rho = |\alpha|^2|0\rangle\langle 0| + |\beta|^2|1\rangle\langle 1|$$

The off-diagonal terms (the quantum coherence) have vanished. The qubit is now just a classical coin — 50/50 with no interference possible.

---

## Types of Errors

### Bit Flip Error
The qubit spontaneously flips: $|0\rangle \to |1\rangle$ or $|1\rangle \to |0\rangle$. This is the quantum analogue of a classical bit flip. Described by the Pauli-$X$ operator acting unexpectedly.

### Phase Flip Error
The relative phase flips: $\alpha|0\rangle + \beta|1\rangle \to \alpha|0\rangle - \beta|1\rangle$. Described by the Pauli-$Z$ operator. This has no classical analogue — classical bits don't have phases.

### Depolarising Error
The qubit is replaced by a completely random state with some probability $p$:

$$\rho \to (1-p)\rho + \frac{p}{3}(X\rho X + Y\rho Y + Z\rho Z)$$

This is the most common noise model used in simulations.

### Amplitude Damping
The qubit spontaneously decays from $|1\rangle$ to $|0\rangle$ — like an excited atom emitting a photon. This is the dominant error in many physical systems.

---

## Coherence Times

Every qubit has two key timescales:

**$T_1$ (relaxation time):** How long before the qubit decays from $|1\rangle$ to $|0\rangle$. Amplitude damping timescale.

**$T_2$ (dephasing time):** How long before the phase relationship between $|0\rangle$ and $|1\rangle$ is lost. Always $T_2 \leq 2T_1$.

Current state-of-the-art:
- Superconducting qubits: $T_1, T_2 \sim 100\ \mu s$ to $1\ ms$
- Trapped ions: $T_1, T_2 \sim$ seconds to minutes
- A single gate takes $\sim 10$–$100\ ns$ for superconducting qubits

So you have a budget of roughly $T_2 / t_{\text{gate}} \sim 10^3$–$10^4$ gates before decoherence destroys your computation. This is why **circuit depth** matters so much.

---

## The Threshold Theorem

Here's the remarkable result that makes quantum computing viable in principle:

> **If the error rate per gate is below a threshold $p_{\text{th}} \approx 1\%$, quantum error correction can suppress errors to arbitrarily low levels.**

This is the **fault-tolerance threshold theorem**. It says: even with noisy hardware, you can do arbitrarily long quantum computations — as long as your error rate is low enough and you use enough physical qubits to encode each logical qubit.

The cost: you need roughly $10^3$–$10^4$ physical qubits per logical qubit. A 1000-logical-qubit computation might need millions of physical qubits. This is why fault-tolerant quantum computing is still a long-term goal.

---

## Noise in Variational Algorithms

[[../Algorithms/Variational Quantum Eigensolver|VQE]] and [[../Algorithms/Quantum Approximate Optimization Algorithm|QAOA]] are designed to work on **noisy** hardware (NISQ — Noisy Intermediate-Scale Quantum devices). They use shallow circuits to minimise decoherence, and the classical optimiser can partially compensate for noise by adjusting parameters.

But noise introduces a fundamental problem: **barren plateaus**. In noisy circuits, gradients vanish exponentially with circuit depth and qubit count — the loss landscape becomes flat and the optimiser can't find a direction to improve. This is the quantum analogue of the vanishing gradient problem in deep networks.

---

> Next: [[Quantum Error Correction]] | Up: [[../QC MOC]]
