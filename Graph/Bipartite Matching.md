## Template
```cpp
struct HopcroftKarp {
    int n, m;
    vector<vector<int>> adj;
    vector<int> pair_u, pair_v, dist;
    const int INF = 1e9;

    HopcroftKarp(int n, int m) : n(n), m(m) {
        adj.resize(n + 1);
        pair_u.assign(n + 1, 0);
        pair_v.assign(m + 1, 0);
        dist.resize(n + 1);
    }

    void add_edge(int u, int v) { // u in 1..n, v in 1..m
        adj[u].push_back(v);
    }

    bool bfs() {
        queue<int> q;
        for (int u = 1; u <= n; u++) {
            if (pair_u[u] == 0) {
                dist[u] = 0;
                q.push(u);
            } else dist[u] = INF;
        }
        dist[0] = INF;
        while (!q.empty()) {
            int u = q.front(); q.pop();
            if (dist[u] < dist[0]) {
                for (int v : adj[u]) {
                    if (dist[pair_v[v]] == INF) {
                        dist[pair_v[v]] = dist[u] + 1;
                        q.push(pair_v[v]);
                    }
                }
            }
        }
        return dist[0] != INF;
    }

    bool dfs(int u) {
        if (u != 0) {
            for (int v : adj[u]) {
                if (dist[pair_v[v]] == dist[u] + 1) {
                    if (dfs(pair_v[v])) {
                        pair_v[v] = u;
                        pair_u[u] = v;
                        return true;
                    }
                }
            }
            dist[u] = INF;
            return false;
        }
        return true;
    }

    int max_matching() {
        int res = 0;
        while (bfs()) {
            for (int u = 1; u <= n; u++) {
                if (pair_u[u] == 0 && dfs(u))
                    res++;
            }
        }
        return res;
    }
};
```
## Complexity
**Time:** $O(E \sqrt{V})$

**Memory:** $O(V + E)$
