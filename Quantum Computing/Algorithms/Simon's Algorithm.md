# Simon's Algorithm

> [[../QC MOC]] | Prev: [[Bernstein-Vazirani Algorithm]] | Next: [[Grover's Algorithm]]

---

## The Problem

You're given $f: \{0,1\}^n \to \{0,1\}^n$ with a promise: there exists a hidden string $s \in \{0,1\}^n$ such that:

$$f(x) = f(y) \iff y = x \oplus s$$

Two cases:
- If $s = 0^n$: $f$ is one-to-one (injective) — every input maps to a unique output
- If $s \neq 0^n$: $f$ is two-to-one — every output has exactly two preimages, $x$ and $x \oplus s$

Your task: find $s$.

**Why is this hard classically?** You need to find two inputs $x, y$ with $f(x) = f(y)$. By the birthday paradox, you'd expect to need $\Omega(2^{n/2})$ queries before a collision. For $n = 128$, that's $2^{64}$ queries — completely infeasible.

**Quantum:** $O(n)$ queries. Exponential speedup.

---

## The Algorithm

Repeat the following $O(n)$ times:

**Step 1:** Prepare $|0\rangle^n|0\rangle^n$

**Step 2:** Apply $H^{\otimes n}$ to the first register:

$$\frac{1}{\sqrt{2^n}}\sum_{x=0}^{2^n-1}|x\rangle|0\rangle^n$$

**Step 3:** Apply the oracle $U_f$:

$$\frac{1}{\sqrt{2^n}}\sum_{x}|x\rangle|f(x)\rangle$$

**Step 4:** Measure the second register. Say you get value $v = f(x_0)$.

The first register collapses to a superposition of all $x$ with $f(x) = v$. Since $f$ is two-to-one (assuming $s \neq 0$), there are exactly two such inputs: $x_0$ and $x_0 \oplus s$:

$$\frac{1}{\sqrt{2}}(|x_0\rangle + |x_0 \oplus s\rangle)$$

**Step 5:** Apply $H^{\otimes n}$ to the first register.

**Step 6:** Measure the first register. You get some string $z$.

---

## The Math of Step 5

Apply $H^{\otimes n}$ to $\frac{1}{\sqrt{2}}(|x_0\rangle + |x_0 \oplus s\rangle)$:

$$\frac{1}{\sqrt{2}} \cdot \frac{1}{\sqrt{2^n}}\sum_z \left[(-1)^{z \cdot x_0} + (-1)^{z \cdot (x_0 \oplus s)}\right]|z\rangle$$

The bracket: $(-1)^{z \cdot x_0}\left[1 + (-1)^{z \cdot s}\right]$

- If $z \cdot s = 0 \pmod{2}$: bracket $= 2(-1)^{z \cdot x_0}$ → non-zero amplitude
- If $z \cdot s = 1 \pmod{2}$: bracket $= 0$ → zero amplitude

**You only ever measure $z$ values satisfying $z \cdot s = 0 \pmod{2}$.**

---

## Solving for $s$

Each run gives you one random $z$ from the set $\{z : z \cdot s = 0\}$. This is a linear constraint on $s$ over $\mathbb{Z}_2$.

After $O(n)$ runs, you have $n-1$ linearly independent equations:

$$z_1 \cdot s = 0$$
$$z_2 \cdot s = 0$$
$$\vdots$$
$$z_{n-1} \cdot s = 0$$

This is a system of linear equations over $\mathbb{Z}_2^n$. Solve it with Gaussian elimination (classical, $O(n^3)$) to find $s$.

**Why $n-1$ equations?** Because the solution space is 2-dimensional: $\{0^n, s\}$. You need $n-1$ independent constraints to narrow it down to these two, then check which is the non-trivial solution.

---

## Why This Matters: The Road to Shor

Simon's algorithm was the direct inspiration for [[Shor's Algorithm]]. The structure is identical:
1. Create superposition over all inputs
2. Apply oracle to encode the function
3. Measure the output register to collapse the input to a periodic state
4. Apply a Fourier transform to extract the period
5. Solve classically

Simon's algorithm works over $\mathbb{Z}_2^n$ (XOR periodicity). Shor's algorithm works over $\mathbb{Z}_N$ (multiplicative periodicity mod $N$) and uses the full [[Quantum Fourier Transform]] instead of Hadamards.

---

> Next: [[Grover's Algorithm]] | Up: [[../QC MOC]]
