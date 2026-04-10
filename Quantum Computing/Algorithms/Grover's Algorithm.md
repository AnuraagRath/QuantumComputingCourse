# Grover's Algorithm

> [[../QC MOC]] | Prev: [[Simon's Algorithm]] | Next: [[Quantum Fourier Transform]]

---

## The Problem

You have an unsorted database of $N = 2^n$ items. Exactly one item satisfies some condition (the "marked" item). Find it.

**Classically:** On average, you need to check $N/2$ items. In the worst case, $N$ items. This is optimal — there's no classical shortcut for an unsorted database.

**Quantum:** Grover's algorithm finds the marked item in $O(\sqrt{N})$ queries. For $N = 10^6$, that's 1000 queries instead of 500,000.

This is a **quadratic speedup** — not exponential like Shor's. But it's provably optimal for quantum search, and it applies to any problem that can be framed as "find the input that satisfies this condition."

---

## The Idea: Amplitude Amplification

Start with a uniform superposition — every item has equal amplitude $\frac{1}{\sqrt{N}}$. The marked item has probability $\frac{1}{N}$ — too small to reliably measure.

Grover's algorithm repeatedly applies two operations that together **rotate the state toward the marked item**. After $\approx \frac{\pi}{4}\sqrt{N}$ iterations, the marked item has amplitude close to 1. Measure and you're done.

---

## The Two Operations

### Operation 1: The Oracle (Phase Flip)

The oracle $U_\omega$ flips the sign of the marked item's amplitude:

$$U_\omega|x\rangle = \begin{cases}-|x\rangle & \text{if } x = \omega \text{ (the marked item)}\\ |x\rangle & \text{otherwise}\end{cases}$$

In matrix form: $U_\omega = I - 2|\omega\rangle\langle\omega|$

This is a **reflection** about the hyperplane perpendicular to $|\omega\rangle$. It doesn't change any probabilities — the marked item still has probability $\frac{1}{N}$. But it flips the sign, setting up the next step.

### Operation 2: The Diffusion Operator (Inversion About the Mean)

$$U_s = 2|s\rangle\langle s| - I, \qquad |s\rangle = \frac{1}{\sqrt{N}}\sum_{x=0}^{N-1}|x\rangle$$

This reflects all amplitudes about their **average**. In matrix form, it's a reflection about the uniform superposition state $|s\rangle$.

**Implementation:** $U_s = H^{\otimes n}(2|0\rangle\langle 0| - I)H^{\otimes n}$

The inner part $2|0\rangle\langle 0| - I$ flips the sign of everything except $|0\rangle$. Sandwiching with Hadamards transforms this into a reflection about $|s\rangle$.

---

## Why Inversion About the Mean Amplifies the Target

Let's trace through one iteration with $N = 4$ items and marked item $\omega = 2$.

**Initial state:** All amplitudes $= \frac{1}{2}$. Mean $= \frac{1}{2}$.

**After oracle:** Marked item amplitude $= -\frac{1}{2}$. Others $= +\frac{1}{2}$. New mean $= \frac{1}{4}(-\frac{1}{2} + \frac{1}{2} + \frac{1}{2} + \frac{1}{2}) = \frac{1}{4}$.

**After diffusion** (each amplitude $a \to 2\mu - a$ where $\mu = \frac{1}{4}$):
- Marked item: $2 \cdot \frac{1}{4} - (-\frac{1}{2}) = \frac{1}{2} + \frac{1}{2} = 1$ ← jumped to 1!
- Others: $2 \cdot \frac{1}{4} - \frac{1}{2} = \frac{1}{2} - \frac{1}{2} = 0$ ← cancelled to 0!

For $N = 4$, one iteration is enough. For larger $N$, the amplification is smaller per step and you need $\approx \frac{\pi}{4}\sqrt{N}$ iterations.

---

## The Geometry: Rotation in 2D

The entire algorithm lives in a 2D subspace spanned by:
- $|\omega\rangle$ — the marked state
- $|s'\rangle = \frac{1}{\sqrt{N-1}}\sum_{x \neq \omega}|x\rangle$ — the uniform superposition over non-marked states

The initial state $|s\rangle$ makes a small angle $\theta$ with $|s'\rangle$:

$$\sin\theta = \langle\omega|s\rangle = \frac{1}{\sqrt{N}} \implies \theta \approx \frac{1}{\sqrt{N}} \text{ for large } N$$

Each Grover iteration is a **rotation by $2\theta$** toward $|\omega\rangle$:
- Oracle: reflection about $|s'\rangle$ (flips the $|\omega\rangle$ component)
- Diffusion: reflection about $|s\rangle$

Two reflections = a rotation. The angle of rotation is $2\theta$.

After $k$ iterations, the state makes angle $(2k+1)\theta$ with $|s'\rangle$. You want this close to $\frac{\pi}{2}$ (pointing at $|\omega\rangle$):

$$(2k+1)\theta \approx \frac{\pi}{2} \implies k \approx \frac{\pi}{4\theta} \approx \frac{\pi}{4}\sqrt{N}$$

**Critical point:** If you apply too many iterations, you rotate past $|\omega\rangle$ and the probability starts decreasing again. Grover's algorithm requires knowing when to stop.

---

## Probability After $k$ Iterations

The probability of measuring the marked item after $k$ iterations is:

$$P_k = \sin^2\left((2k+1)\theta\right), \qquad \sin\theta = \frac{1}{\sqrt{N}}$$

This oscillates. The maximum is at $k^* = \lfloor\frac{\pi}{4}\sqrt{N}\rfloor$, where $P_{k^*} \geq 1 - \frac{1}{N}$.

---

## Multiple Marked Items

If there are $M$ marked items out of $N$, the optimal number of iterations is:

$$k^* \approx \frac{\pi}{4}\sqrt{\frac{N}{M}}$$

If $M = N/4$: only $\frac{\pi}{4} \cdot 2 \approx 1.6$ iterations needed. If you know $M$, you can tune the algorithm. If you don't know $M$, there are quantum counting algorithms to estimate it first.

---

## Applications Beyond Database Search

Grover's algorithm applies to any problem where you can write an oracle that recognises a solution:
- **Satisfiability (SAT):** Oracle checks if a bitstring satisfies a Boolean formula
- **Collision finding:** Find two inputs with the same hash (quadratic speedup over birthday attack)
- **Optimisation:** Find the minimum of a function by searching for inputs below a threshold
- **Cryptography:** Grover halves the effective key length of symmetric ciphers (AES-128 becomes AES-64 equivalent) — this is why post-quantum cryptography recommends doubling key sizes

---

> Next: [[Quantum Fourier Transform]] | Up: [[../QC MOC]]
