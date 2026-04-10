# Measurement and Collapse

> [[../QC MOC]] | Prev: [[Quantum Gates]] | Next: [[Entanglement]]

---

## The Fundamental Asymmetry

In classical computing, reading a bit is free and harmless. You can check the value of a bit a million times and it stays the same.

In quantum computing, **measuring a qubit is a destructive, irreversible act.** The moment you look at a qubit in superposition, the superposition is gone. The qubit "picks" one value and stays there. You can never recover the original superposition.

This is not a limitation of our instruments. It's a fundamental feature of quantum mechanics. And it's the central constraint that makes quantum algorithm design hard.

---

## What Happens When You Measure

Take a qubit in state:

$$|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$$

When you measure it in the **computational basis** ($\{|0\rangle, |1\rangle\}$):

- With probability $|\alpha|^2$: you get outcome **0**, and the state **collapses** to $|0\rangle$
- With probability $|\beta|^2$: you get outcome **1**, and the state **collapses** to $|1\rangle$

After measurement, the qubit is in a definite state. The superposition is destroyed. If you measure again immediately, you'll get the same outcome — the qubit is no longer in superposition.

---

## The Born Rule

The rule connecting amplitudes to probabilities is called the **Born rule** (after physicist Max Born):

$$P(\text{outcome } x) = |\langle x|\psi\rangle|^2$$

Let's unpack this. $\langle x|\psi\rangle$ is the inner product between the basis state $|x\rangle$ and the current state $|\psi\rangle$. It's a complex number. The Born rule says: square its magnitude to get the probability.

**Why squared magnitude?** Because amplitudes can be complex, and probabilities must be real and non-negative. The squared magnitude $|z|^2 = z \cdot z^*$ is always real and non-negative for any complex number $z$.

**Example:** State $|\psi\rangle = \frac{1}{\sqrt{2}}|0\rangle - \frac{i}{\sqrt{2}}|1\rangle$

$$P(0) = \left|\frac{1}{\sqrt{2}}\right|^2 = \frac{1}{2}, \qquad P(1) = \left|\frac{-i}{\sqrt{2}}\right|^2 = \frac{|-i|^2}{2} = \frac{1}{2}$$

Even though the amplitude of $|1\rangle$ is imaginary ($-i/\sqrt{2}$), the probability is still $\frac{1}{2}$. The imaginary part affects phase and interference, not measurement probabilities directly.

---

## Projective Measurement: The General Framework

The computational basis measurement is a special case of a more general framework. A **projective measurement** is defined by a set of projection operators $\{P_m\}$ satisfying:

$$\sum_m P_m = I \qquad \text{(completeness)}$$
$$P_m P_{m'} = \delta_{mm'} P_m \qquad \text{(orthogonality)}$$

When you measure and get outcome $m$:
1. The probability of that outcome is: $P(m) = \langle\psi|P_m|\psi\rangle$
2. The state after measurement is: $|\psi'\rangle = \dfrac{P_m|\psi\rangle}{\sqrt{P(m)}}$

The denominator renormalises the state so it's still a unit vector.

**For computational basis measurement:** $P_0 = |0\rangle\langle 0|$ and $P_1 = |1\rangle\langle 1|$.

$$P_0|\psi\rangle = |0\rangle\langle 0|(\alpha|0\rangle + \beta|1\rangle) = \alpha|0\rangle$$

After normalising: $\frac{\alpha|0\rangle}{|\alpha|} = e^{i\phi}|0\rangle \approx |0\rangle$ (global phase doesn't matter).

This is exactly a **projection** in the linear algebra sense — you're projecting the state vector onto a subspace and renormalising. Same concept as in [[../../Mathematics of ML and DL/Foundations/Linear Algebra/Eigenvalues and Eigenvectors]].

---

## Measuring in Different Bases

You don't have to measure in the $\{|0\rangle, |1\rangle\}$ basis. You can measure in any orthonormal basis.

**Example — measuring in the $\{|+\rangle, |-\rangle\}$ basis:**

$$|+\rangle = \frac{|0\rangle+|1\rangle}{\sqrt{2}}, \qquad |-\rangle = \frac{|0\rangle-|1\rangle}{\sqrt{2}}$$

If your state is $|+\rangle$, measuring in the $\{|+\rangle, |-\rangle\}$ basis gives $|+\rangle$ with certainty. But measuring in the $\{|0\rangle, |1\rangle\}$ basis gives 50/50.

The choice of measurement basis is part of the algorithm design. Shor's algorithm, for instance, uses the QFT to rotate into a basis where the answer is concentrated.

---

## Partial Measurement on Multi-Qubit Systems

With multiple qubits, you can measure just one and leave the others alone. This is where things get interesting.

Consider the 2-qubit state:

$$|\psi\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$$

Measure the first qubit:
- Get $0$ (probability $\frac{1}{2}$): state collapses to $|00\rangle$ — the second qubit is now definitely 0
- Get $1$ (probability $\frac{1}{2}$): state collapses to $|11\rangle$ — the second qubit is now definitely 1

You measured the first qubit, but the second qubit's state was instantly determined. This is **entanglement** — the two qubits are correlated in a way that has no classical explanation. (More in [[Entanglement]].)

---

## Why This Constrains Algorithm Design

You only get **one shot**. Measure too early and you collapse the superposition, losing all quantum advantage. Measure too late and decoherence (see [[Quantum Noise and Decoherence]]) destroys the state anyway.

The goal of every quantum algorithm is to engineer the state so that by the time you measure, the amplitude of the correct answer is close to 1 — all other amplitudes have been cancelled by [[Interference]]. Then measurement is almost certain to give you the right answer.

---

> Next: [[Entanglement]] | Up: [[../QC MOC]]
