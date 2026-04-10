# The NISQ Era — Where We Actually Are

> [[../QC MOC]]

---

## What is NISQ?

**NISQ** — Noisy Intermediate-Scale Quantum — is the term coined by John Preskill in 2018 to describe the quantum computers we have right now and in the near future:

- **Noisy:** Gate error rates of ~0.1–1%. No fault tolerance. Errors accumulate.
- **Intermediate-Scale:** 50–1000 physical qubits. Enough to be interesting, not enough to run fault-tolerant algorithms.

We are firmly in the NISQ era. Understanding what this means is essential for knowing what you can actually do with Qiskit today.

---

## The Gap Between Theory and Practice

| Algorithm | Qubits needed (logical) | Physical qubits needed | Status |
|---|---|---|---|
| Grover's (database of $2^{20}$) | 20 | ~20,000 (with QEC) | Not yet |
| Shor's (RSA-2048) | ~4,000 | ~4,000,000 | Far future |
| QPE for H₂ energy | ~10 | ~10,000 | Not yet |
| VQE for H₂ | 4 | 4 (noisy) | **Now** |
| QAOA for small graphs | 10–50 | 10–50 (noisy) | **Now** |
| Quantum simulation (small) | 10–50 | 10–50 (noisy) | **Now** |

The algorithms with proven exponential speedups (Shor, QPE-based chemistry) all require fault tolerance — millions of physical qubits. NISQ devices can't run them.

---

## What NISQ Devices Can Do

**Definitely useful now:**
- Quantum simulation of small physical systems (condensed matter, chemistry)
- Benchmarking and characterising quantum hardware
- Quantum error correction research (small codes)
- Quantum communication (BB84 is deployed commercially)

**Potentially useful (active research):**
- VQE for quantum chemistry (small molecules)
- QAOA for combinatorial optimisation (unclear if quantum advantage exists)
- Quantum machine learning (very unclear — most proposed speedups have caveats)
- Quantum sensing (not computing, but uses quantum effects)

**Not useful on NISQ:**
- Cryptography (Shor's needs fault tolerance)
- Large-scale search (Grover's needs deep circuits)
- Solving large linear systems (HHL needs fault tolerance)

---

## The Fault-Tolerance Roadmap

IBM's public roadmap (approximate):

| Year | Milestone |
|---|---|
| 2023 | 1000+ physical qubits (Condor, 1121 qubits) |
| 2024–2025 | Error correction demonstrations, ~10 logical qubits |
| 2026–2028 | 100 logical qubits, early fault-tolerant algorithms |
| 2030+ | Thousands of logical qubits, cryptographically relevant |

Other players: Google (Willow chip, 2024 — demonstrated below-threshold error correction), IonQ (trapped ions, fewer but higher-quality qubits), Quantinuum, Rigetti.

---

## Quantum Advantage: Has It Been Achieved?

**Google's 2019 claim (Sycamore):** Solved a specific sampling problem in 200 seconds that would take a classical supercomputer 10,000 years. Later disputed — classical algorithms improved significantly.

**Google's 2024 (Willow):** Demonstrated that error rates decrease as you add more qubits (below the fault-tolerance threshold). A major milestone for error correction, not a practical application.

**Honest assessment:** No quantum computer has demonstrated a practical, useful quantum advantage over the best classical algorithms on a problem anyone actually cares about. This is the current state of the field.

---

## What This Means for Your Qiskit Work

1. **Use simulators for development.** Real hardware is slow, noisy, and has queue times. Develop and test on `AerSimulator`, then run small experiments on real hardware.

2. **Keep circuits shallow.** Every gate adds noise. Aim for circuit depth < 100 on current hardware.

3. **Use error mitigation.** Qiskit Runtime's `resilience_level=1` or `2` significantly improves results at modest cost.

4. **Be sceptical of quantum ML claims.** Most proposed quantum speedups for ML have caveats (data loading, output readout) that eliminate the advantage.

5. **Focus on quantum simulation.** This is the most principled near-term application — quantum computers simulating quantum systems is what Feynman envisioned, and it's where real advantage is most likely first.

---

> Up: [[../QC MOC]]
