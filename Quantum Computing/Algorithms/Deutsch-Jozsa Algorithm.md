# Deutsch-Jozsa Algorithm

> [[../QC MOC]] | Prev: [[../Foundations/Phase Kickback]] | Next: [[Bernstein-Vazirani Algorithm]]

---

## The Problem

You're given a black-box function $f: \{0,1\}^n \to \{0,1\}$. You're promised it's one of two types:
- **Constant:** $f(x)$ is the same for all inputs — either always 0 or always 1
- **Balanced:** $f(x) = 0$ for exactly half the inputs and $f(x) = 1$ for the other half

Your task: determine which type it is.

**Classically:** In the worst case, you need to query $f$ on $2^{n-1} + 1$ inputs. Why? Because if you've seen $2^{n-1}$ outputs all equal to 0, the function could still be balanced (the remaining half might all be 1) or constant (all outputs are 0). You need one more query to be certain.

**Quantum:** You need exactly **1 query**. Always. Regardless of $n$.

This was the first proof that quantum computers can be exponentially faster than classical ones.

---

## The Circuit

```
|0⟩^n ──[H^⊗n]──────[U_f]──────[H^⊗n]──[M]
|1⟩   ──[H]──────────[   ]──────
```

Five steps:
1. Prepare $n$ input qubits in $|0\rangle^{\otimes n}$ and one ancilla qubit in $|1\rangle$
2. Apply Hadamard to **all** qubits (including the ancilla)
3. Apply the oracle $U_f$
4. Apply Hadamard to the **input** qubits only
5. Measure the input register

---

## Step-by-Step Walkthrough

### Step 1: Initial State

$$|\psi_0\rangle = |0\rangle^{\otimes n} \otimes |1\rangle$$

### Step 2: Apply Hadamard to Everything

Hadamard on the $n$ input qubits creates uniform superposition:

$$H^{\otimes n}|0\rangle^{\otimes n} = \frac{1}{\sqrt{2^n}}\sum_{x=0}^{2^n-1}|x\rangle$$

Hadamard on the ancilla $|1\rangle$ creates $|-\rangle$:

$$H|1\rangle = \frac{|0\rangle - |1\rangle}{\sqrt{2}} = |-\rangle$$

Combined state after step 2:

$$|\psi_1\rangle = \frac{1}{\sqrt{2^n}}\sum_{x=0}^{2^n-1}|x\rangle \otimes |-\rangle$$

### Step 3: Apply the Oracle (Phase Kickback)

The oracle $U_f|x\rangle|y\rangle = |x\rangle|y \oplus f(x)\rangle$ acts on each $|x\rangle|-\rangle$ term. By [[../Foundations/Phase Kickback|phase kickback]]:

$$U_f|x\rangle|-\rangle = (-1)^{f(x)}|x\rangle|-\rangle$$

So the full state becomes:

$$|\psi_2\rangle = \frac{1}{\sqrt{2^n}}\sum_{x=0}^{2^n-1}(-1)^{f(x)}|x\rangle \otimes |-\rangle$$

The ancilla is unchanged. The function $f$ has been encoded into the **phases** of the input register.

### Step 4: Apply Hadamard Again to Input

The Hadamard transform on $n$ qubits maps:

$$H^{\otimes n}|x\rangle = \frac{1}{\sqrt{2^n}}\sum_{z=0}^{2^n-1}(-1)^{x \cdot z}|z\rangle$$

where $x \cdot z = \sum_i x_i z_i \pmod{2}$ is the bitwise dot product.

Applying this to $|\psi_2\rangle$:

$$|\psi_3\rangle = \frac{1}{\sqrt{2^n}}\sum_x (-1)^{f(x)} \cdot \frac{1}{\sqrt{2^n}}\sum_z (-1)^{x \cdot z}|z\rangle$$

$$= \sum_z \left[\frac{1}{2^n}\sum_x (-1)^{f(x) + x \cdot z}\right]|z\rangle$$

The amplitude of measuring $|z\rangle$ is the term in brackets.

### Step 5: Measure — What Do You Get?

**The amplitude of measuring $|0\rangle^{\otimes n}$ (i.e., $z = 0$):**

When $z = 0$: $x \cdot z = 0$ for all $x$, so:

$$\text{amplitude of }|0\rangle^n = \frac{1}{2^n}\sum_{x=0}^{2^n-1}(-1)^{f(x)}$$

Now:
- If $f$ is **constant** (all $f(x) = 0$): sum $= \frac{1}{2^n} \cdot 2^n \cdot 1 = 1$. You measure $|0\rangle^n$ with **certainty**.
- If $f$ is **constant** (all $f(x) = 1$): sum $= \frac{1}{2^n} \cdot 2^n \cdot (-1) = -1$. Amplitude $= -1$, probability $= |-1|^2 = 1$. Still measure $|0\rangle^n$ with certainty.
- If $f$ is **balanced**: exactly $2^{n-1}$ terms are $+1$ and $2^{n-1}$ terms are $-1$. Sum $= 0$. You **never** measure $|0\rangle^n$.

**Conclusion:** Measure the input register. If you get all zeros → constant. If you get anything else → balanced. One query, done.

---

## Why This Works: The Intuition

The Hadamard transform is a Fourier transform over $\mathbb{Z}_2^n$. The second Hadamard layer computes the "Fourier transform" of the phase pattern $(-1)^{f(x)}$.

- A constant function has a flat phase pattern → its Fourier transform is a delta function at $z=0$ → all amplitude concentrates at $|0\rangle^n$
- A balanced function has a zero-sum phase pattern → its Fourier transform has zero component at $z=0$ → no amplitude at $|0\rangle^n$

The algorithm is detecting whether the phase pattern is flat (constant) or zero-sum (balanced) using interference.

---

> Next: [[Bernstein-Vazirani Algorithm]] | Up: [[../QC MOC]]
