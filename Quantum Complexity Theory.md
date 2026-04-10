# Quantum Complexity Theory

> [[../QC MOC]] | Foundations: [[../Foundations/Qubits and Superposition]]

---

## Why This Matters

Before you write a single line of Qiskit, you need to know: **when does a quantum computer actually help?**

The answer is not "always." Quantum computers are not universally faster. They're faster for a specific, well-defined class of problems. Complexity theory tells you exactly which class that is — and why.

---

## Classical Complexity: A Quick Recap

**P** — problems solvable in polynomial time on a classical computer. Sorting, shortest path, matrix multiplication. "Easy."

**NP** — problems where a solution can be *verified* in polynomial time, but finding one might take exponential time. SAT, graph colouring, travelling salesman. "Hard to solve, easy to check."

**P ⊆ NP** — every easy problem is also easy to verify. Whether P = NP is the most famous open problem in mathematics.

---

## BQP: The Quantum Complexity Class

**BQP** (Bounded-error Quantum Polynomial time) is the class of problems a quantum computer can solve in polynomial time with error probability $\leq \frac{1}{3}$.

"Bounded-error" means: the algorithm might be wrong, but with probability at most $\frac{1}{3}$. You can reduce this to $\frac{1}{2^k}$ by repeating $k$ times and taking the majority vote.

**Known relationships:**

$$P \subseteq BPP \subseteq BQP \subseteq PSPACE$$

- **BPP** — classical probabilistic polynomial time (randomised algorithms). Quantum computers are at least as powerful.
- **PSPACE** — problems solvable with polynomial *space* (but possibly exponential time). Quantum computers can't solve everything in PSPACE efficiently.
- **BQP vs NP** — unknown. It's believed $NP \not\subseteq BQP$, meaning quantum computers probably can't solve NP-hard problems efficiently. Grover gives a quadratic speedup for NP problems, but not polynomial.

---

## What's in BQP but Not Known to Be in P?

These are the problems where quantum computers give a provable or strongly suspected speedup:

| Problem | Classical best | Quantum | Algorithm |
|---|---|---|---|
| Integer factoring | Sub-exponential $e^{O(n^{1/3})}$ | Polynomial $O(n^3)$ | Shor's |
| Discrete logarithm | Sub-exponential | Polynomial | Shor's variant |
| Unstructured search | $O(N)$ | $O(\sqrt{N})$ | Grover's |
| Simulating quantum systems | Exponential | Polynomial | Quantum simulation |
| Solving linear systems $Ax=b$ | $O(N^{2.37})$ | $O(\log N)$* | HHL |

*HHL has caveats — see [[../Algorithms/HHL Algorithm]].

---

## QMA: The Quantum Analogue of NP

**QMA** (Quantum Merlin-Arthur) is the quantum analogue of NP. A problem is in QMA if:
- A quantum "proof" (a quantum state) can be verified in polynomial quantum time
- If no solution exists, no quantum state can fool the verifier

The canonical QMA-complete problem: **Local Hamiltonian Problem** — given a Hamiltonian $H = \sum_i H_i$ where each $H_i$ acts on $k$ qubits, is the ground state energy below a threshold? This is what VQE tries to solve approximately.

---

## The Quantum Speedup Landscape

```
Problems quantum computers DEFINITELY help with:
  → Factoring, discrete log (Shor) — exponential speedup
  → Quantum simulation — exponential speedup
  → Unstructured search (Grover) — quadratic speedup
  → Linear systems (HHL) — exponential speedup (with caveats)

Problems quantum computers PROBABLY don't help with:
  → NP-complete problems (SAT, TSP) — only Grover's √N speedup
  → General optimisation — heuristic improvement at best (QAOA)

Problems quantum computers DEFINITELY don't help with:
  → Problems outside PSPACE
  → Problems where the bottleneck is I/O, not computation
```

---

## The Oracle Separation

Many quantum speedups are proven in the **oracle model** — where the algorithm can query a black-box function. Deutsch-Jozsa, Bernstein-Vazirani, Simon's, and Grover's are all oracle algorithms.

Oracle separations prove that quantum computers are more powerful than classical ones *relative to an oracle* — but this doesn't directly imply real-world speedups. Shor's algorithm is special because it achieves an exponential speedup for a *concrete* problem (factoring), not just an oracle problem.

---

## Why This Matters for Qiskit

When you're designing a quantum algorithm in Qiskit, ask:
1. Is my problem in BQP? (Does it have quantum structure to exploit?)
2. Is there a known quantum algorithm, or am I using a heuristic (VQE/QAOA)?
3. What's the actual speedup — exponential, quadratic, or just heuristic?
4. Does the speedup survive noise and error correction overhead?

Most near-term Qiskit applications are heuristic (VQE, QAOA) — no proven speedup, but potentially useful in practice.

---

> Up: [[../QC MOC]]
