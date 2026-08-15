## 1. Core Algebraic Properties
* **Identity:** $A \oplus 0 = A$
* **Self-Inverse:** $A \oplus A = 0$
* **Commutative:** $A \oplus B = B \oplus A$
* **Associative:** $(A \oplus B) \oplus C = A \oplus (B \oplus C)$
* **Bitwise Inversion:** $A \oplus ALL\ ONES = \sim A$
* **Self-Reversibility:** If $C = A \oplus B$, then:
	* $A = C \oplus B$
	* $B = C \oplus A$
---
## 2. Arithmetic Conversions (XOR, Addition, Subtraction)

### Addition Equivalence
* $$A + B = (A \oplus B) + 2 \times (A \ \text{AND} \ B)$$
* $$A + B = (A \ \text{OR} \ B) + (A \ \text{AND} \ B)$$
* $$A + B = (A \oplus B) + 2 \times (A \ \text{AND} \ \sim B)$$
 *(When carries are shifted)*
### Subtraction Equivalence
* $$A - B = (A \oplus B) - 2 \times (\sim A \ \text{AND} \ B)$$
* $$A - B = (A \ \text{AND} \ \sim B) - (\sim A \ \text{AND} \ B)$$
### Deriving Bitwise Operations from Standard Arithmetic
* **XOR:** 
$$A \oplus B = (A + B) - 2 \times (A \ \text{AND} \ B)$$
* **AND:** 
$$A \ \text{AND} \ B = \frac{(A + B) - (A \oplus B)}{2}$$
* **OR:** 
$$A \ \text{OR} \ B = \frac{(A + B) + (A \oplus B)}{2}$$
### Special Condition: Addition Without Carry
* $$A + B = A \oplus B \iff (A \ \text{AND} \ B) = 0$$
**NOTE:** $\iff$ means if and only if
---
## 3. Fundamental Bitwise Hacks & Tricks

### Swap Two Variables Without Temp

$$a = a \oplus b$$
$$b = a \oplus b \quad \text{(b gets original a)}$$
$$a = a \oplus b \quad \text{(a gets original b)}$$
### Toggle Bitwise State (Flip Specific Bits)

* **Flip all bits:** $x \oplus ALL\ ONES$ (or $\sim x$)
* **Flip $k$-th bit (0-indexed):** $x \oplus (1 \ll k)$
### Rightmost Bit Manipulations

* **Toggle rightmost set bit:** $n \ \text{AND} \ (n - 1)$
* **Isolate rightmost set bit:** $n \ \text{AND} \ (-n)$ or $n \ \text{AND} \ \sim (n - 1)$
* **Isolate rightmost zero bit:** $\sim n \ \text{AND} \ (n + 1)$
---
## 4. Range XOR Trick & Sequential XOR ($0$ to $N$)
### Range XOR Formula

$$\text{XOR}(L \dots R) = \text{PrefixXOR}(R) \oplus \text{PrefixXOR}(L - 1)$$
### Fast Sequential XOR Sum from $0$ to $N$: $f(N)$
$$f(N) =  \begin{cases}  N & \text{if } N \pmod 4 = 0 \\ 1 & \text{if } N \pmod 4 = 1 \\ N + 1 & \text{if } N \pmod 4 = 2 \\ 0 & \text{if } N \pmod 4 = 3  \end{cases}$$

---

## 5. Ready-to-Use Code Snippets

### Software Addition (Without using `+` operator)

To add two numbers without the `+` operator, we use $\oplus$ for the sum without carry, and $\text{AND}$ with a left shift ($\ll$) for the carry.

```cpp
int add(int a, int b) {
    while (b != 0) {
        // Calculate carry
        int carry = (a & b) << 1;
        // Sum without carry
        a = a ^ b;
        // Add carry in the next iteration
        b = carry;
    }
    return a;
}

```

### B. Pairwise XOR Sum $\sum \sum (A[i] \oplus A[j])$ in $\mathcal{O}(N \log A)$

For each bit $b$:

* $c_1 =$ count of elements with bit $b$ set
* $c_0 = N - c_1$
* $$\text{Sum} \mathrel{+}= (c_1 \times c_0) \times (1 \ll b)$$