## Template
```cpp
struct SparseTable {
    vector<vector<int>> table;
    vector<int> lg;
    int n, pw;

    SparseTable(vector<int>&v) {
        n = v.size();

        lg.assign(n + 1, 0);
        for ( int i = 2 ; i <= n ; i++ )
            lg[i] = lg[i >> 1] + 1;

        pw = lg[n] + 1;
        table.assign(pw, vector<int>(n));
        for ( int i = 0 ; i < n ; i++ )
            table[0][i] = v[i];

        for ( int j = 1; j < pw ; j++ )
            for ( int i = 0 ; i + (1 << j) - 1 < n ; i++ )
                table[j][i] = merge(table[j-1][i], table[j-1][i + (1 << (j-1))]);
    }

    int merge(int a, int b) {
        return (a & b);
    }

    int query(int l, int r) {
        int lg_sz = lg[r - l + 1];
        return merge(table[lg_sz][l], table[lg_sz][r - (1 << lg_sz) + 1]);
    }
};
```

## Template (Yasser)
```cpp
class sparse
{
private:
    ll n;
    vector<array<ll, 32>> ans;
    ll merge(ll a, ll b)
    {
        return gcd(a, b);
    }
    void build()
    {
        ll len = __lg(n);

        for (ll j = 1; j <= len; j++)
        {
            for (ll i = 0; i + (1LL << (j - 1)) < n; i++)
            {
                ans[i][j] = merge(ans[i][j - 1], ans[i + (1LL << (j - 1))][j - 1]);
            }
        }
    }

public:
    ll query(ll l, ll r)
    {
        ll len = __lg(r - l + 1);
        return merge(ans[l][len], ans[r - (1LL << len) + 1][len]);
    }
    sparse(vector<ll> &a)
    {
        n = a.size();
        ans.resize(n);
        for (ll i = 0; i < n; i++)
        {
            ans[i][0] = a[i];
        }
        build();
    }
};
```
## Complexity
**Time:** 
- **build:** $O(\text{N} \log_2 \text{N})$
- **query:** $O(1)$

**Memory:** $O(\text{N} \log_2 \text{N})$