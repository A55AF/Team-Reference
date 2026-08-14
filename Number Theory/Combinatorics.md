## Mod Space & Fast Power

```cpp
const int mod = 1e9 + 7;

// Valid and fast in most cases (a, b <= 1e9)
ll add(ll a, ll b) {
    return (a + b + mod) % mod;
}
ll sub(ll a, ll b) { 
	return ((a - b) % mod + mod) % mod;
}
ll mul(ll a, ll b) {
    return (a * b) % mod;
}

// More Safe but slow (use it with very large numbers)
ll add(ll a, ll b) {
    return ((a % mod) + (b % mod)) % mod;
}
ll sub(ll a, ll b) {
	return (((a % mod) - (b % mod)) % mod + mod) % mod;
}
ll mul(ll a, ll b) {
    return ((a % mod) * (b % mod)) % mod;
}


ll fpow(ll b, ll pw)
{
    ll ans = 1;
    while (pw > 0)
    {
        if (pw % 2 == 1)
            ans = 1LL * mul(ans, b);
        b = mul(b, b);
        pw = pw / 2;
    }
    return ans;
}
ll inv(ll a) {
	return fpow(a, mod - 2);
}
ll divis(ll a, ll b) {
	return mul(a, inv(b));
}
```

## Permutations & Combinations

```cpp
int fact[N];
void factorial() {
    fact[0] = fact[1] = 1;
    for (int i = 2; i < N; i++)
        fact[i] = mul(fact[i - 1], i);
}
ll nPr (ll n, ll r) {
    if (r < 0 || n < r) return 0;
    return divis(fact[n], fact[n - r]);
}
ll nCr (ll n, ll r) {
    if (r < 0 || n < r) return 0;
    return divis(fact[n], mul(fact[r], fact[n - r]));
}
ll snb(ll n, ll k) {
	if (k == 0) return (n == 0) ? 1 : 0;
	return nCr(n + k - 1, k - 1);
}
```