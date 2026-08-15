## Template
```cpp
const ll N = 1e5 + 5;
const ll B = 3;
const int mod = 1e9 + 7;
ll bases[B] = {31, 43, 57};
ll pw[B][N + 5];
ll inv[B][N + 5];
#define multihash array<ll, B>
ll mul(ll a, ll b) { return ((a % mod) * (b % mod) + mod + mod) % mod; }
ll add(ll a, ll b) { return ((a % mod) + (b % mod) + mod + mod) % mod; }
ll fp(ll a, ll b) {
    if (b == 0)
        return 1;
    ll hp = fp(a, b / 2);
    hp = mul(hp, hp);
    return (b & 1) ? mul(a, hp) : hp;
}
ll invv(ll a) { return fp(a, mod - 2); }
class Hash {
public:
    ll hashChar(char c, ll idx, ll bas) {
        return mul((c - 'a' + 1), pw[bas][idx]);
    }
    vector<multihash> hsh;
    Hash(string &s) {
        hsh.resize(s.size() + 1);
        ll sz = s.size();
        for (ll bas = 0; bas < B; bas++) {
            hsh[0][bas] = 0;
            for (ll i = 1; i <= sz; i++) {
                hsh[i][bas] = add(hsh[i - 1][bas], hashChar(s[i - 1], i, bas));
            }
        }
    }

    multihash getHash(ll l, ll r) {
        multihash ret;
        // 0-based
        l++, r++;
        for (ll bas = 0; bas < B; bas++) {
            ret[bas] = mul(add(hsh[r][bas], -hsh[l - 1][bas]), inv[bas][l - 1]);
        }
        return ret;
    }

    multihash merge(ll l1, ll r1, ll l2, ll r2) {
        multihash ret;
        // 0-based
        l1++, r1++, l2++, r2++;
        for (ll bas = 0; bas < B; bas++) {
            ret[bas] =
                mul(add(hsh[r1][bas], -hsh[l1 - 1][bas]), inv[bas][l1 - 1]);
        }

        for (ll bas = 0; bas < B; bas++) {
            ret[bas] = add(
                ret[bas],
                mul(mul(add(hsh[r2][bas], -hsh[l2 - 1][bas]), inv[bas][l2 - 1]),
                    pw[bas][r1 - l1 + 1]));
        }

        return ret;
    }
};
void pre() {
    for (int bas = 0; bas < B; bas++) {
        pw[bas][0] = 1;
        inv[bas][0] = invv(1);
        for (int i = 1; i <= N; i++) {
            pw[bas][i] = mul(pw[bas][i - 1], bases[bas]);
            inv[bas][i] = invv(pw[bas][i]);
        }
    }
}
```

## Template (Assaf)
```cpp
#define multihash array<ll, H>
const int H = 2, N = 3e6+7;
vector<multihash> pw(N), inv(N);
const multihash base = {311, 337};
const multihash mod = {(ll)1e9+7, (ll)1e9+9};
ll add(ll a, ll b, int type) {
    return (a + b + mod[type]) % mod[type];
}
ll mul(ll a, ll b, int type) {
    return (a * b) % mod[type];
}
ll fpow(ll b, ll pw, int type) {
    ll ans = 1;
    while (pw > 0) {
        if (pw % 2 == 1)
            ans = 1LL * mul(ans, b, type);
        b = mul(b, b, type);
        pw = pw / 2;
    }
    return ans;
}
ll inverse(ll x, int type) { return fpow(x, mod[type] - 2, type); }
void precompute() {
    pw[0][0] = pw[0][1] = inv[0][0] = inv[0][1] = 1;
    for ( int b = 0 ; b < H ; b++ ) {
        ll div = inverse(base[b], b);
        for ( int i = 1 ; i < N ; i++ ) {
            pw[i][b] = mul(pw[i-1][b], base[b], b);
            inv[i][b] = mul(inv[i-1][b], div, b);
        }
    }
}
struct hstring {
    vector<multihash> hash;
    hstring(const string&s) {
        int n = s.size();
        hash = vector<multihash>(n+1);
        for ( int b = 0 ; b < H ; b++ ) {
            for ( int i = 1 ; i <= n ; i++ ) {
                ll char_id = (unsigned char)s[i - 1];
                hash[i][b] = mul(char_id, pw[i][b], b);
                hash[i][b] = add(hash[i][b], hash[i-1][b], b);
            }
        }
    }
    multihash range(ll l, ll r) { // 1-Based
        multihash ans;
        for ( int b = 0; b < H ; b++ )
            ans[b] = mul(add(hash[r][b], -hash[l-1][b], b), inv[l-1][b], b);
        return ans;
    }
};
```

## Custom Hash
```cpp
struct custom_hash {
    static uint64_t splitmix64(uint64_t x) {
        x += 0x9e3779b97f4a7c15;
        x = (x ^ (x >> 30)) * 0xbf58476d1ce4e5b9;
        x = (x ^ (x >> 27)) * 0x94d049bb133111eb;
        return x ^ (x >> 31);
    }

    size_t operator()(uint64_t x) const {
        static const uint64_t FIXED_RANDOM =
            chrono::steady_clock::now().time_since_epoch().count();
        return splitmix64(x + FIXED_RANDOM);
    }
};
```

## Pair Custom Hash
```cpp
struct custom_hash {
    static uint64_t splitmix64(uint64_t x) {
        x += 0x9e3779b97f4a7c15;
        x = (x ^ (x >> 30)) * 0xbf58476d1ce4e5b9;
        x = (x ^ (x >> 27)) * 0x94d049bb133111eb;
        return x ^ (x >> 31);
    }

    template <typename T1, typename T2>
    size_t operator()(const std::pair<T1, T2> &p) const {
        static const uint64_t FIXED_RANDOM =
            std::chrono::steady_clock::now().time_since_epoch().count();

        uint64_t h1 = splitmix64(std::hash<T1>{}(p.first) + FIXED_RANDOM);
        uint64_t h2 = splitmix64(std::hash<T2>{}(p.second) + FIXED_RANDOM);

        return h1 ^ (h2 + 0x9e3779b9 + (h1 << 6) + (h1 >> 2));
    }
};
```
## Complexity
**Time:** 
- **build:** $O(N)$
- **query:** $O(1)$

**Memory:** $O(N)$
