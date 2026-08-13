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
## Complexity
**Time:** 
- **build:** $O(\text{N} \log_2 \text{N})$
- **query:** $O(1)$

**Memory:** $O(\text{N} \log_2 \text{N})$