# Quantum Approximate Optimization Algorithm (QAOA)

> [[../QC MOC]] | Prev: [[Variational Quantum Eigensolver]]

---

## The Problem: Combinatorial Optimisation

Many real-world problems are **combinatorial optimisation** problems: find the best assignment of binary variables $x_1, x_2, \ldots, x_n \in \{0,1\}$ that maximises (or minimises) some objective function.

Examples:
- **Max-Cut:** Partition a graph's vertices into two sets to maximise the number of edges between sets
- **Travelling Salesman:** Find the shortest route visiting all cities
- **Portfolio Optimisation:** Choose which assets to hold to maximise return for a given risk
- **Drug Discovery:** Find the molecular configuration with lowest energy

These problems are NP-hard in general — no known polynomial-time classical algorithm. QAOA is a quantum heuristic that may find good approximate solutions faster.

---

## Encoding the Problem as a Hamiltonian

The first step is to encode the optimisation problem as a quantum Hamiltonian $H_C$ (the "cost Hamiltonian") whose ground state encodes the optimal solution.

**Example: Max-Cut on graph $G = (V, E)$**

Assign each vertex $i$ a qubit. The variable $x_i \in \{0,1\}$ determines which partition vertex $i$ belongs to. An edge $(i,j)$ is "cut" if $x_i \neq x_j$.

The number of cut edges is:

$$C(x) = \sum_{(i,j) \in E} \frac{1 - (-1)^{x_i + x_j}}{2} = \sum_{(i,j) \in E} \frac{1 - (-1)^{x_i}(-1)^{x_j}}{2}$$

In quantum mechanics, $(-1)^{x_i}$ corresponds to the Pauli-$Z$ operator (eigenvalue $+1$ for $|0\rangle$, $-1$ for $|1\rangle$). So the cost Hamiltonian is:

$$H_C = \frac{1}{2}\sum_{(i,j) \in E}(I - Z_i Z_j)$$

The ground state of $H_C$ (the state with minimum energy = maximum $-C$) is the Max-Cut solution.

---

## The QAOA Circuit

QAOA uses two alternating unitaries for $p$ rounds:

$$|\psi(\gamma, \beta)\rangle = \underbrace{e^{-i\beta_p H_B}e^{-i\gamma_p H_C}}_{\text{round }p} \cdots \underbrace{e^{-i\beta_1 H_B}e^{-i\gamma_1 H_C}}_{\text{round }1} |+\rangle^{\otimes n}$$

where:
- $|+\rangle^{\otimes n} = H^{\otimes n}|0\rangle^{\otimes n}$ — uniform superposition over all $2^n$ solutions
- $e^{-i\gamma H_C}$ — the **phase separator**: applies phases based on the cost
- $e^{-i\beta H_B}$ — the **mixer**: $H_B = \sum_i X_i$, mixes the state to explore solutions
- $\gamma = (\gamma_1, \ldots, \gamma_p)$ and $\beta = (\beta_1, \ldots, \beta_p)$ — tunable parameters

---

## What Each Unitary Does

### Phase Separator $e^{-i\gamma H_C}$

For Max-Cut, $H_C = \frac{1}{2}\sum_{(i,j)}(I - Z_iZ_j)$. The unitary is:

$$e^{-i\gamma H_C} = \prod_{(i,j) \in E} e^{i\gamma Z_i Z_j / 2}$$

(up to a global phase). Each factor is a two-qubit gate that applies a phase rotation based on whether qubits $i$ and $j$ agree or disagree:

$$e^{i\gamma Z_i Z_j/2}|x_i x_j\rangle = \begin{cases}e^{i\gamma/2}|x_i x_j\rangle & \text{if } x_i = x_j \\ e^{-i\gamma/2}|x_i x_j\rangle & \text{if } x_i \neq x_j\end{cases}$$

States that cut more edges accumulate more phase — this is how the cost function is encoded into the quantum state.

### Mixer $e^{-i\beta H_B}$

$$e^{-i\beta H_B} = \prod_i e^{-i\beta X_i} = \prod_i R_x(2\beta)$$

This applies a rotation around the X-axis to each qubit independently. It "mixes" the state — spreading amplitude from high-cost states to nearby states. Without the mixer, the algorithm would get stuck.

The mixer plays the role of **momentum** in gradient descent: it prevents the algorithm from getting trapped in local optima by exploring the neighbourhood.

---

## The Optimisation Loop

Same as [[Variational Quantum Eigensolver|VQE]]:

1. Prepare $|\psi(\gamma, \beta)\rangle$ on the quantum computer
2. Measure to estimate $\langle H_C \rangle = \langle\psi(\gamma,\beta)|H_C|\psi(\gamma,\beta)\rangle$ — the expected cost
3. Classical optimiser updates $(\gamma, \beta)$ to increase $\langle H_C \rangle$
4. Repeat until convergence
5. Sample the final state — with high probability, get a good solution

---

## Why Alternating Unitaries?

This structure comes from **Trotterisation** — a way to simulate the time evolution under $H_C + H_B$ by alternating small steps:

$$e^{-i(H_C + H_B)t} \approx \left(e^{-i H_C t/p} e^{-i H_B t/p}\right)^p$$

As $p \to \infty$, this becomes exact. QAOA with $p$ layers is a $p$-step Trotterisation of the adiabatic quantum computation that continuously interpolates from $H_B$ (easy ground state = $|+\rangle^n$) to $H_C$ (hard ground state = optimal solution).

**Adiabatic theorem:** If you change a Hamiltonian slowly enough, a system in the ground state stays in the ground state. QAOA is a discretised, parameterised version of this adiabatic process.

---

## Performance

At depth $p = 1$, QAOA gives a provable approximation ratio for Max-Cut:
- Guaranteed to find a cut with at least $0.6924 \times \text{OPT}$ edges cut (Farhi et al.)
- The best classical polynomial-time algorithm (Goemans-Williamson) achieves $0.878 \times \text{OPT}$

So QAOA at $p=1$ is actually worse than the best classical algorithm. As $p$ increases, QAOA improves — but so does the circuit depth and the number of parameters to optimise.

Whether QAOA achieves a quantum advantage over classical algorithms for practical problem sizes is an open research question.

---

## QAOA vs Classical Optimisation

| | QAOA | Simulated Annealing |
|---|---|---|
| State | Quantum superposition of all solutions | Single solution |
| Exploration | Quantum interference + mixing | Random perturbations |
| Gradient | Parameter shift rule | Finite differences |
| Convergence | Approximate, depth-dependent | Approximate, temperature-dependent |
| Parallelism | All solutions simultaneously | One solution at a time |

---

> Up: [[../QC MOC]]
