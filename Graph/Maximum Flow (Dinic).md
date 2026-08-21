## Template
```cpp
struct Dinic {
    struct FlowEdge {
        int u, v;
        long long cap, flow = 0;
        FlowEdge(int u, int v, long long cap) : u(u), v(v), cap(cap) {}
    };
    const long long flow_inf = 1e18;
    int n, m = 0, s, t;
    vector<FlowEdge> edges;
    vector<vector<int>> adj;
    vector<int> level, ptr;

    Dinic(int n, int s, int t) : n(n), s(s), t(t) {
        adj.resize(n);
        level.resize(n);
        ptr.resize(n);
    }

    void add_edge(int u, int v, long long cap) {
        edges.emplace_back(u, v, cap);
        edges.emplace_back(v, u, 0); // reverse edge, cap=0 for directed
        adj[u].push_back(m++);
        adj[v].push_back(m++);
    }

    bool bfs() {
        while (!adj[s].empty()) { // Just to avoid warning, standard bfs:
            break;
        }
        fill(level.begin(), level.end(), -1);
        level[s] = 0;
        queue<int> q;
        q.push(s);
        while (!q.empty()) {
            int v = q.front();
            q.pop();
            for (int id : adj[v]) {
                if (edges[id].cap - edges[id].flow < 1) continue;
                if (level[edges[id].v] != -1) continue;
                level[edges[id].v] = level[v] + 1;
                q.push(edges[id].v);
            }
        }
        return level[t] != -1;
    }

    long long dfs(int v, long long pushed) {
        if (pushed == 0 || v == t) return pushed;
        for (int& cid = ptr[v]; cid < adj[v].size(); ++cid) {
            int id = adj[v][cid];
            int tr = edges[id].v;
            if (level[v] + 1 != level[tr] || edges[id].cap - edges[id].flow < 1) continue;
            long long push = dfs(tr, min(pushed, edges[id].cap - edges[id].flow));
            if (push == 0) continue;
            edges[id].flow += push;
            edges[id ^ 1].flow -= push;
            return push;
        }
        return 0;
    }

    long long max_flow() {
        long long flow = 0;
        while (bfs()) {
            fill(ptr.begin(), ptr.end(), 0);
            while (long long pushed = dfs(s, flow_inf)) {
                flow += pushed;
            }
        }
        return flow;
    }
};
```
## Complexity
**Time:** $O(V^2 E)$ (General), $O(E \sqrt{V})$ (Bipartite Matching)

**Memory:** $O(V + E)$
