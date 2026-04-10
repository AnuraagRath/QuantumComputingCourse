# Shor's Algorithm

> [[../QC MOC]] | Prev: [[Quantum Fourier Transform]] | Next: [[Variational Quantum Eigensolver]]

---

## Why This Algorithm Changed Everything

In 1994, Peter Shor published an algorithm that could factor large integers in polynomial time on a quantum computer. This was a bombshell.

RSA encryption — used to secure virtually all internet traffic — relies on the fact that multiplying two large primes is easy, but factoring their product is computationally infeasible. The best classical factoring algorithm (the General Number Field Sieve) runs in sub-exponential time $e^{O(n^{1/3}(\log n)^{2/3})}$ where $n$ is the number of bits. For a 2048-bit number, this is completely infeasible.

Shor's algorithm runs in $O(n^3)$ — polynomial. A sufficiently large quantum computer would break RSA.

---

## The Key Insight: Factoring Reduces to Period Finding

This reduction is **classical number theory** — no quantum mechanics yet.

**Claim:** If you can find the **order** $r$ of a random integer $a$ modulo $N$ — the smallest positive integer $r$ such that:

$$a^r \equiv 1 \pmod{N}$$

— then you can factor $N$ with high probability.

**Why?** Because $a^r - 1 \equiv 0 \pmod{N}$, which means $N | (a^r - 1) = (a^{r/2} - 1)(a^{r/2} + 1)$.

If $r$ is even and $a^{r/2} \not\equiv \pm 1 \pmod{N}$, then:
- $\gcd(a^{r/2} - 1, N)$ is a non-trivial factor of $N$
- $\gcd(a^{r/2} + 1, N)$ is a non-trivial factor of $N$

It can be shown that a random $a$ satisfies these conditions with probability $\geq \frac{1}{2}$.

**So the problem reduces to:** Given $a$ and $N$, find the period $r$ of the function $f(x) = a^x \bmod N$.

---

## The Quantum Part: Period Finding

The function $f(x) = a^x \bmod N$ is periodic with period $r$:

$$f(x) = f(x + r) \text{ for all } x$$

We need to find $r$. Classically, this requires $\Omega(\sqrt{r})$ evaluations of $f$ (birthday paradox). Quantum mechanically, the QFT can find $r$ in $O(n^2)$ gates.

### Step 1: Create Superposition

Prepare two registers: input register with $2n$ qubits (to get enough precision), output register with $n$ qubits.

$$|0\rangle^{2n}|0\rangle^n \xrightarrow{H^{\otimes 2n} \otimes I} \frac{1}{\sqrt{2^{2n}}}\sum_{x=0}^{2^{2n}-1}|x\rangle|0\rangle^n$$

### Step 2: Compute $f(x)$ in the Output Register

Apply the modular exponentiation oracle:

$$\frac{1}{\sqrt{2^{2n}}}\sum_{x=0}^{2^{2n}-1}|x\rangle|a^x \bmod N\rangle$$

This is the expensive step — computing $a^x \bmod N$ for a superposition of all $x$ simultaneously. The circuit uses repeated squaring and requires $O(n^3)$ gates.

### Step 3: Measure the Output Register

Say you measure value $v = a^{x_0} \bmod N$. The input register collapses to a superposition of all $x$ with $a^x \equiv v \pmod{N}$:

$$\frac{1}{\sqrt{M}}\sum_{j=0}^{M-1}|x_0 + jr\rangle$$

where $M = \lfloor(2^{2n} - x_0)/r\rfloor$. This is a **periodic state** with period $r$ — exactly the kind of state the QFT is designed to analyse.

### Step 4: Apply the QFT

Apply the [[Quantum Fourier Transform]] to the input register. As shown in the QFT note, this concentrates amplitude at multiples of $\frac{2^{2n}}{r}$.

### Step 5: Measure the Input Register

You get some value $k \approx m \cdot \frac{2^{2n}}{r}$ for a random integer $m$.

### Step 6: Extract $r$ (Classical)

Compute $\frac{k}{2^{2n}} \approx \frac{m}{r}$. Apply the **continued fractions algorithm** to find the fraction $\frac{m}{r}$ in lowest terms. The denominator is $r$ (or a divisor of $r$).

Repeat a few times if needed to get the correct $r$.

---

## The Full Algorithm

```
1. Pick random a with 1 < a < N
2. Compute gcd(a, N) — if > 1, found a factor (done)
3. [QUANTUM] Find the order r of a mod N using QFT
4. If r is odd or a^(r/2) ≡ -1 (mod N): go to step 1
5. Compute gcd(a^(r/2) ± 1, N) — these are the factors
```

Steps 1, 2, 4, 5 are classical and fast. Step 3 is the quantum subroutine.

---

## Complexity

| Step | Classical cost | Quantum cost |
|---|---|---|
| Modular exponentiation circuit | — | $O(n^3)$ gates |
| QFT | — | $O(n^2)$ gates |
| Continued fractions | $O(n^3)$ | — |
| Total | $O(n^3)$ | $O(n^3)$ gates |

Compare to the best classical factoring: $e^{O(n^{1/3})}$ — exponentially worse.

---

## Why the QFT Works Here

The function $f(x) = a^x \bmod N$ is periodic. The QFT is a Fourier transform. The Fourier transform of a periodic function is peaked at the fundamental frequency and its harmonics. Measuring after the QFT gives you the frequency, from which you extract the period.

This is the same principle as using FFT to find the frequency of a periodic signal in signal processing — just done on quantum amplitudes, exponentially faster.

---

## Real-World Status

Shor's algorithm has been demonstrated on small quantum computers (factoring 15 = 3×5, 21 = 3×7, etc.). To factor RSA-2048, you'd need roughly:
- ~4,000 logical qubits
- ~$10^7$ physical qubits (with error correction)
- ~8 hours of computation

Current quantum computers have ~1000 noisy physical qubits. We're still years away from cryptographically relevant quantum computers — but the threat is taken seriously enough that NIST has already standardised post-quantum cryptographic algorithms.

---

> Next: [[Variational Quantum Eigensolver]] | Up: [[../QC MOC]]
