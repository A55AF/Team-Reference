## Template
```cpp
// SOS DP: Compute for all mask: F[mask] = sum_{i in mask} A[i]
void sos_dp(vector<long long>& a, int n_bits) {
    int max_mask = 1 << n_bits;
    vector<long long> f(max_mask);
    
    // Base Initialization
    for (int mask = 0; mask < max_mask; ++mask)
        f[mask] = a[mask];

    // Iterative DP
    for (int i = 0; i < n_bits; ++i) {
        for (int mask = 0; mask < max_mask; ++mask) {
            if (mask & (1 << i)) {
                // mask has i-th bit set, so add contribution from subset without i-th bit
                f[mask] += f[mask ^ (1 << i)];
            }
        }
    }
}
```
## Complexity
**Time:** $O(N \cdot 2^N)$

**Memory:** $O(2^N)$
