## Template
```cpp
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

// Finds one valid solution for a*x + b*y = c
bool find_any_solution(ll a, ll b, ll c, ll &x0, ll &y0, ll &g) {
    g = extGCD(abs(a), abs(b), x0, y0);
    if (c % g != 0) return false;

    x0 *= c / g;
    y0 *= c / g;
    if (a < 0) x0 = -x0;
    if (b < 0) y0 = -y0;
    return true;
}
```
## Complexity
**Time:** $O(\log(\min(A, B)))$

**Memory:** $O(1)$
