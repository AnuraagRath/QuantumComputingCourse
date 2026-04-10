# Bernstein-Vazirani Algorithm

> [[../QC MOC]] | Prev: [[Deutsch-Jozsa Algorithm]] | Next: [[Simon's Algorithm]]

---

## The Problem

There's a hidden bitstring $s \in \{0,1\}^n$. You're given a black-box function:

$$f(x) = s \cdot x \pmod{2} = s_1 x_1 \oplus s_2 x_2 \oplus \cdots \oplus s_n x_n$$

This is the **bitwise dot product** of $s$ and $x$, taken mod 2. For example, if $s = 101$ and $x = 110$:

$$f(110) = 1 \cdot 1 \oplus 0 \cdot 1 \oplus 1 \cdot 0 = 1 \oplus 0 \oplus 0 = 1$$

Your task: find $s$.

**Classically:** Query $f$ on each standard basis vector $e_i = 00\ldots010\ldots0$ (a 1 in position $i$, zeros elsewhere):

$$f(e_i) = s \cdot e_i = s_i$$

This reveals one bit of $s$ per query. You need $n$ queries to find all $n$ bits.

**Quantum:** You need exactly **1 query**.

---

## The Circuit

Identical structure to [[Deutsch-Jozsa Algorithm]]:

```
|0⟩^n ──[H^⊗n]──[U_f]──[H^⊗n]──[M]
|1⟩   ──[H]────[     ]──
```

---

## Step-by-Step Walkthrough

### After Hadamard + Oracle (Phase Kickback)

Following the same steps as Deutsch-Jozsa, after the oracle the state is:

$$\frac{1}{\sqrt{2^n}}\sum_{x=0}^{2^n-1}(-1)^{f(x)}|x\rangle \otimes |-\rangle = \frac{1}{\sqrt{2^n}}\sum_{x}(-1)^{s \cdot x}|x\rangle \otimes |-\rangle$$

### After the Second Hadamard

The amplitude of measuring $|z\rangle$ is:

$$\frac{1}{2^n}\sum_{x=0}^{2^n-1}(-1)^{s \cdot x + z \cdot x} = \frac{1}{2^n}\sum_{x}(-1)^{(s \oplus z) \cdot x}$$

This sum is a key identity: $\frac{1}{2^n}\sum_x (-1)^{c \cdot x} = \begin{cases}1 & \text{if } c = 0^n \\ 0 & \text{if } c \neq 0^n\end{cases}$

**Why?** If $c \neq 0$, there's at least one bit where $c_i = 1$. Summing $(-1)^{c \cdot x}$ over all $x$ gives equal numbers of $+1$ and $-1$ terms (for every $x$, flipping bit $i$ flips the sign) — they cancel. If $c = 0$, all terms are $(-1)^0 = 1$ and the sum is $2^n$.

So the amplitude of $|z\rangle$ is:
- $1$ if $s \oplus z = 0$, i.e., $z = s$
- $0$ otherwise

**You measure $s$ with probability 1.** One query, all $n$ bits of $s$ revealed simultaneously.

---

## Why This is More Than a Trick

The classical algorithm queries $f$ on $n$ carefully chosen inputs to extract $s$ one bit at a time. The quantum algorithm queries $f$ on a **superposition of all $2^n$ inputs simultaneously**, and the interference pattern in the output directly encodes $s$.

The function $f(x) = s \cdot x$ is linear over $\mathbb{Z}_2^n$. The Hadamard transform is the Fourier transform over $\mathbb{Z}_2^n$. The Fourier transform of a linear function is a delta function — all amplitude concentrates at the "frequency" $s$. The algorithm is computing this Fourier transform in one shot.

This is the same idea as [[Shor's Algorithm]], just over a simpler group.

---

> Next: [[Simon's Algorithm]] | Up: [[../QC MOC]]
