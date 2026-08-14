### 1. Differences & Ranges
- GCD Difference Invariance:  $gcd(a, b, c) = gcd(a, b - a, c - b)$
  -> Enables Range Add + Range GCD via Segment Tree on difference array.
- Logarithmic Subarray Bound: Subarray GCD can change at most log2(MaxA) times.

### 2. Duality & Algebraic Identities
- Distributive Law:           $gcd(a, lcm(b, c)) = lcm(gcd(a, b), gcd(a, c))$
- Fibonacci / Power GCDs:     $gcd(F_a, F_b) = F_{gcd(a,b)}$
                              $gcd(x^a - 1, x^b - 1) = x^{gcd(a,b)} - 1$

### 3. Pairwise GCD / LCM Formulas
- Pairs with LCM = N:         Count = $∏ (2*e_i + 1)$ for N = $∏ (p_i ^ {e_i})$
- Pairs with GCD = g:         Use Inclusion-Exclusion backwards from MaxA.

```cpp
vector<long long> cnt_div(MAX_VAL + 1, 0);
for (int i = 1; i <= MAX_VAL; i++) {
	if (freq[i] == 0) continue;
	for (int j = i; j <= MAX_VAL; j += i) {
		cnt_div[j] += freq[i];
	}
}
```