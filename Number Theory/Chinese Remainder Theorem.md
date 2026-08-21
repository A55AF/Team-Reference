## Template
```cpp
// Returns (g, x, y) such that a*x + b*y = g = gcd(a, b)
ll extGCD(ll a, ll b, ll &x, ll &y) {
    if (b == 0) {
        x = 1; y = 0;
        return a;
    }
    ll x1, y1;
    ll d = extGCD(b, a % b, x1, y1);
    x = y1;
    y = x1 - y1 * (a / b);
    return d;
}

// Solves x = a1 (mod m1) and x = a2 (mod m2)
// Returns {x, lcm(m1, m2)}. Returns {-1, -1} if no solution.
pair<ll, ll> CRT2(ll a1, ll m1, ll a2, ll m2) {
    ll x, y;
    ll g = extGCD(m1, m2, x, y);
    if ((a1 - a2) % g != 0) return {-1, -1};
    
    ll lcm = (m1 / g) * m2;
    ll mod = m2 / g;
    ll k = (x % mod + mod) % mod;
    k = (k * (((a2 - a1) / g) % mod + mod)) % mod;
    
    ll ans = (a1 + k * m1) % lcm;
    ans = (ans + lcm) % lcm;
    return {ans, lcm};
}

// General CRT for array of congruences
pair<ll, ll> CRT(const vector<ll>& a, const vector<ll>& m) {
    ll ans = a[0], lcm = m[0];
    for (int i = 1; i < a.size(); i++) {
        pair<ll, ll> p = CRT2(ans, lcm, a[i], m[i]);
        if (p.first == -1) return {-1, -1}; // No solution
        ans = p.first;
        lcm = p.second;
    }
    return {ans, lcm};
}
```
## Complexity
**Time:** $O(K \log(\text{lcm}))$

**Memory:** $O(K)$
