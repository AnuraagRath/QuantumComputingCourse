# Interference

> [[../QC MOC]] | Prev: [[Quantum Circuits]] | Next: [[Phase Kickback]]

---

## The Real Mechanism Behind Quantum Speedup

Here's the most common misconception about quantum computing: "a quantum computer tries all answers simultaneously and picks the right one."

This is wrong. If it were true, you'd just measure immediately after creating a superposition and get the answer. But measurement gives you a *random* outcome — you'd just get a random input, not the right answer.

The actual mechanism is **interference**. Quantum algorithms are carefully engineered so that:
- Paths leading to **wrong answers** have amplitudes that **cancel** (destructive interference)
- Paths leading to **right answers** have amplitudes that **add up** (constructive interference)

By the time you measure, the right answer has amplitude close to 1. Everything else has been cancelled away.

---

## What is Interference?

Interference is what happens when two waves overlap. It's not unique to quantum mechanics — it happens with water waves, sound waves, light waves.

- **Constructive interference:** Two waves in phase → they add → bigger wave
- **Destructive interference:** Two waves out of phase → they cancel → no wave

In quantum mechanics, the "waves" are probability amplitudes. Because amplitudes can be **negative** (and complex), they can cancel each other out.

---

## A Concrete Example: Double Hadamard

Start with $|0\rangle$. Apply $H$, then apply $H$ again. What do you get?

**Step 1:** $H|0\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$

**Step 2:** Apply $H$ to this superposition:

$$H\left(\frac{|0\rangle + |1\rangle}{\sqrt{2}}\right) = \frac{1}{\sqrt{2}}\left(H|0\rangle + H|1\rangle\right)$$

$$= \frac{1}{\sqrt{2}}\left(\frac{|0\rangle+|1\rangle}{\sqrt{2}} + \frac{|0\rangle-|1\rangle}{\sqrt{2}}\right)$$

$$= \frac{1}{2}\left[(1+1)|0\rangle + (1-1)|1\rangle\right] = \frac{1}{2}[2|0\rangle + 0|1\rangle] = |0\rangle$$

You're back to $|0\rangle$. Why?

- The $|0\rangle$ amplitude got contributions $+\frac{1}{2}$ and $+\frac{1}{2}$ → they added → amplitude $= 1$
- The $|1\rangle$ amplitude got contributions $+\frac{1}{2}$ and $-\frac{1}{2}$ → they cancelled → amplitude $= 0$

The $|1\rangle$ path **destructively interfered** with itself. The $|0\rangle$ path **constructively interfered**. This is interference doing computational work.

---

## Phase is the Key

Interference requires amplitudes to have different signs (or more generally, different phases). This is why:

1. **Amplitudes must be complex** — real numbers can be negative, but complex numbers give you a full circle of phases ($e^{i\theta}$ for any $\theta$), enabling much richer interference patterns.

2. **Phase gates matter** — the $Z$, $S$, $T$ gates don't change measurement probabilities, but they change phases, which changes how states interfere. They're the "tuning knobs" of interference.

3. **Oracles encode information in phases** — the [[Phase Kickback]] trick writes $f(x)$ into the phase of $|x\rangle$ as $(-1)^{f(x)}$. Then interference can distinguish $f(x)=0$ from $f(x)=1$ by whether amplitudes add or cancel.

---

## The Algorithm Template

Every quantum algorithm follows this skeleton:

```
1. PREPARE: Create uniform superposition over all inputs
            |0⟩^n  →  H^⊗n  →  (1/√2^n) Σ|x⟩

2. ORACLE: Encode the problem into phases
           Σ|x⟩  →  U_f  →  Σ(-1)^f(x)|x⟩

3. INTERFERE: Apply more gates to amplify right answers,
              cancel wrong answers

4. MEASURE: With high probability, get the right answer
```

Step 3 is the hard part — and it's different for every algorithm:
- [[../Algorithms/Deutsch-Jozsa Algorithm]]: one Hadamard layer — all wrong answers cancel in one shot
- [[../Algorithms/Grover's Algorithm]]: repeated "inversion about the mean" — gradually amplifies the target
- [[../Algorithms/Quantum Fourier Transform]]: interference in the frequency domain — reveals periodicity

---

## Why Classical Computers Can't Do This

A classical probabilistic computer also has "weights" on different states — but they're probabilities, always $\geq 0$. You can't have destructive interference with probabilities:

$$p_1 + p_2 \geq 0 \text{ always}$$

Quantum amplitudes can be negative or complex. This means:

$$\alpha_1 + \alpha_2 = 0 \text{ is possible}$$

This is the fundamental difference. Quantum computing is not just "probabilistic computing with more states." The ability to cancel amplitudes — to make certain outcomes *impossible* — is uniquely quantum.

---

> Next: [[Phase Kickback]] | Up: [[../QC MOC]]
