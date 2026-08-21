## Template
```cpp
// General form: dp[i][j] = min_{k < j} (dp[i-1][k] + C(k, j))
// Requires cost function C to satisfy quadrangle inequality

vector<long long> dp_prev, dp_curr;

long long cost(int i, int j) {
    // Return cost of segment [i+1, j]
    return 0;
}

void compute(int l, int r, int optl, int optr) {
    if (l > r) return;
    int mid = l + (r - l) / 2;
    pair<long long, int> best = {1e18, -1};

    for (int k = optl; k <= min(mid, optr); k++) {
        long long val = dp_prev[k] + cost(k, mid);
        if (val < best.first) {
            best = {val, k};
        }
    }

    dp_curr[mid] = best.first;
    int opt = best.second;

    compute(l, mid - 1, optl, opt);
    compute(mid + 1, r, opt, optr);
}

void solve_dc(int k, int n) {
    dp_prev.assign(n + 1, 1e18);
    dp_curr.assign(n + 1, 1e18);
    dp_prev[0] = 0; // base case

    for (int i = 1; i <= k; i++) {
        compute(1, n, 0, n);
        dp_prev = dp_curr;
    }
}
```
## Complexity
**Time:** $O(K \cdot N \log N)$

**Memory:** $O(N \cdot K)$
