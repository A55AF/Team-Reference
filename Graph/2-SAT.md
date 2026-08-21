## Template
```cpp
struct TwoSat {
    int n;
    vector<vector<int>> adj, adj_rev;
    vector<bool> used;
    vector<int> order, comp;
    vector<bool> assignment;

    TwoSat(int n) : n(n) { // n variables (0 to n-1)
        adj.assign(2 * n, vector<int>());
        adj_rev.assign(2 * n, vector<int>());
    }

    void add_edge(int u, int v) { // internally used
        adj[u].push_back(v);
        adj_rev[v].push_back(u);
    }

    // var i is true: 2*i, false: 2*i + 1
    void add_disjunction(int a, bool na, int b, bool nb) {
        a = 2 * a + na; // na = 0 means true, na = 1 means negation
        b = 2 * b + nb;
        int neg_a = a ^ 1, neg_b = b ^ 1;
        add_edge(neg_a, b);
        add_edge(neg_b, a);
    }

    void dfs1(int v) {
        used[v] = true;
        for (int u : adj[v])
            if (!used[u]) dfs1(u);
        order.push_back(v);
    }

    void dfs2(int v, int cl) {
        comp[v] = cl;
        for (int u : adj_rev[v])
            if (comp[u] == -1) dfs2(u, cl);
    }

    bool solve() {
        used.assign(2 * n, false);
        for (int i = 0; i < 2 * n; ++i)
            if (!used[i]) dfs1(i);

        comp.assign(2 * n, -1);
        int j = 0;
        for (int i = 2 * n - 1; i >= 0; --i) {
            int v = order[i];
            if (comp[v] == -1) dfs2(v, j++);
        }

        assignment.assign(n, false);
        for (int i = 0; i < n; ++i) {
            if (comp[2 * i] == comp[2 * i + 1])
                return false; // Contradiction
            assignment[i] = comp[2 * i] > comp[2 * i + 1];
        }
        return true;
    }
};
```
## Complexity
**Time:** $O(V + E)$

**Memory:** $O(V + E)$
