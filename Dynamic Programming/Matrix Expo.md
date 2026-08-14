## Template
```cpp
const ll LG =25;
ll getBit(ll num, ll idx)
{
    return (num >> idx) & 1;
}
struct matrix {
    vector<vector<ll>> mat;
    ll n, m;

    matrix(ll ni, ll mi, ll num = 0) {
        mat.resize(ni, vector<ll>(mi, num));
        n = ni;
        m = mi;
    }
};

matrix matmul(matrix &a, matrix &b) {
    matrix c(a.n, b.m);
    for (ll i = 0; i < a.n; i++) {
        for (ll j = 0; j < b.m; j++) {
            c.mat[i][j] = 0;
            for (ll k = 0; k < a.m; k++) {
                c.mat[i][j] = add(c.mat[i][j], mul(a.mat[i][k], b.mat[k][j]));
            }
        }
    }
    return c;
}
vector<matrix> mat_pre;
bool pre_done = 0;
void pre(matrix &base) {
    pre_done = 1;
    vector<matrix> mat_pre_(LG, matrix(base.n, base.m));
    mat_pre = mat_pre_;
    mat_pre[0] = base;
    for (ll i = 1; i < LG; i++) {
        mat_pre[i] = matmul(mat_pre[i - 1], mat_pre[i - 1]);
    }
}
matrix matpow(matrix &base, long long n) {
    matrix ans(base.n, base.m);
    for (ll i = 0; i < base.n; i++) {
        ans.mat[i][i] = 1;
    }
    if (n <= 1) {
        return (n == 1) ? base : ans;
    }
    if (!pre_done) {
        pre(base);
    }

    for (ll i = 0; i < 32; i++) {
        if (getBit(n, i)) {
            ans = matmul(ans, mat_pre[i]);
        }
    }

    // while (n)
    // {
    //     if (n & 1)
    //     {
    //         ans = matmul(ans, base);
    //     }
    //     base = matmul(base, base);
    //     n >>= 1;
    // }
    return ans;
}
```

**NOTE: It depends on the modular arithmetic functions, add them from [Combinatorics](Number%20Theory/Combinatorics.md)**