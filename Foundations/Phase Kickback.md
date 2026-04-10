# Phase Kickback

> [[../QC MOC]] | Prev: [[Interference]] | Next: [[Quantum Noise and Decoherence]]

---

## The Most Important Trick in Quantum Algorithms

Phase kickback is the mechanism that almost every quantum algorithm relies on. It's how you encode information about a function $f(x)$ into the quantum state in a way that interference can act on.

Without phase kickback, you can't do Deutsch-Jozsa, Bernstein-Vazirani, Simon's, Grover's, or Shor's algorithm. It's that fundamental.

---

## The Setup

You have two registers:
- **Input register:** $|x\rangle$ — the state you want to process
- **Ancilla register:** a single qubit in the state $|-\rangle = \frac{|0\rangle - |1\rangle}{\sqrt{2}}$

You apply an oracle $U_f$ that computes $f(x)$:

$$U_f|x\rangle|y\rangle = |x\rangle|y \oplus f(x)\rangle$$

---

## What Happens

Apply $U_f$ to $|x\rangle|-\rangle$:

$$U_f|x\rangle|-\rangle = U_f|x\rangle \cdot \frac{|0\rangle - |1\rangle}{\sqrt{2}}$$

$$= |x\rangle \cdot \frac{|0 \oplus f(x)\rangle - |1 \oplus f(x)\rangle}{\sqrt{2}}$$

Now consider the two cases:

**Case 1: $f(x) = 0$**

$$|x\rangle \cdot \frac{|0\rangle - |1\rangle}{\sqrt{2}} = |x\rangle|-\rangle$$

Nothing changed.

**Case 2: $f(x) = 1$**

$$|x\rangle \cdot \frac{|1\rangle - |0\rangle}{\sqrt{2}} = |x\rangle \cdot \left(-\frac{|0\rangle - |1\rangle}{\sqrt{2}}\right) = -|x\rangle|-\rangle$$

A global minus sign appeared on $|x\rangle$.

**Combining both cases:**

$$U_f|x\rangle|-\rangle = (-1)^{f(x)}|x\rangle|-\rangle$$

---

## Why This is Remarkable

Look at what happened:
- The ancilla $|-\rangle$ is **completely unchanged** — it's still $|-\rangle$ in both cases
- The information about $f(x)$ has "kicked back" into the **phase** of the input register $|x\rangle$
- If $f(x) = 0$: phase is $+1$ (no change)
- If $f(x) = 1$: phase is $-1$ (sign flip)

The oracle was supposed to write $f(x)$ into the ancilla. Instead, it wrote $f(x)$ into the phase of the input. The ancilla is a "catalyst" — it enables the kickback but is unchanged itself.

---

## Why Phase Matters

A phase of $(-1)^{f(x)}$ is invisible to measurement — $|(-1)^{f(x)} \cdot \alpha|^2 = |\alpha|^2$. So why is this useful?

Because when you have a **superposition** of inputs:

$$\frac{1}{\sqrt{2^n}}\sum_x |x\rangle \xrightarrow{U_f \text{ with } |-\rangle} \frac{1}{\sqrt{2^n}}\sum_x (-1)^{f(x)}|x\rangle$$

Now different $|x\rangle$ states have different phases. When you apply more gates (like another layer of Hadamards), these phases cause **constructive and destructive interference**. States where $f(x) = 0$ and $f(x) = 1$ interfere differently, and the result encodes information about $f$ that you can extract by measurement.

---

## Concrete Example: Deutsch's Problem

$f: \{0,1\} \to \{0,1\}$. Is $f$ constant or balanced?

Start: $|0\rangle|-\rangle$

After $H$ on input: $\frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)|-\rangle$

After oracle (phase kickback):

$$\frac{1}{\sqrt{2}}((-1)^{f(0)}|0\rangle + (-1)^{f(1)}|1\rangle)|-\rangle$$

After $H$ on input:

- If $f$ is **constant** ($f(0) = f(1)$): both phases are the same, say both $+1$. After $H$: you get $|0\rangle$. Measure: always 0.
- If $f$ is **balanced** ($f(0) \neq f(1)$): phases are $+1$ and $-1$. After $H$: you get $|1\rangle$. Measure: always 1.

One query. The phase kickback encoded the answer into the interference pattern.

---

> Next: [[Quantum Noise and Decoherence]] | Up: [[../QC MOC]]
