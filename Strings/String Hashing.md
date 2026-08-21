## Template
```cpp
const int MOD1 = 1e9 + 7;
const int MOD2 = 1e9 + 9;
const int P1 = 313;
const int P2 = 317;

struct Hash {
    vector<long long> h1, h2;
    vector<long long> p1, p2;
    int n;

    Hash(string s) {
        n = s.size();
        h1.assign(n + 1, 0); h2.assign(n + 1, 0);
        p1.assign(n + 1, 1); p2.assign(n + 1, 1);
        for (int i = 0; i < n; i++) {
            h1[i + 1] = (h1[i] * P1 + s[i]) % MOD1;
            h2[i + 1] = (h2[i] * P2 + s[i]) % MOD2;
            p1[i + 1] = (p1[i] * P1) % MOD1;
            p2[i + 1] = (p2[i] * P2) % MOD2;
        }
    }

    pair<long long, long long> query(int l, int r) { // 0-indexed, inclusive
        long long hash1 = (h1[r + 1] - (h1[l] * p1[r - l + 1]) % MOD1 + MOD1) % MOD1;
        long long hash2 = (h2[r + 1] - (h2[l] * p2[r - l + 1]) % MOD2 + MOD2) % MOD2;
        return {hash1, hash2};
    }
};
```
## Complexity
**Time:** 
 - **build:** $O(N)$
 - **query:** $O(1)$

**Memory:** $O(N)$
