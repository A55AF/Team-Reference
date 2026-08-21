## Template
```cpp
int n;
vector<vector<int>> adj;
vector<int> tin, low;
int timer;
vector<bool> is_bridge, is_articulation;
vector<pair<int, int>> bridges;

void dfs(int v, int p = -1) {
    tin[v] = low[v] = timer++;
    int children = 0;
    for (int to : adj[v]) {
        if (to == p) continue;
        if (tin[to] != -1) {
            low[v] = min(low[v], tin[to]);
        } else {
            dfs(to, v);
            low[v] = min(low[v], low[to]);
            if (low[to] > tin[v]) {
                // edge v-to is a bridge
                bridges.push_back({v, to});
                is_bridge[to] = true; // mapped to child node
            }
            if (low[to] >= tin[v] && p != -1)
                is_articulation[v] = true;
            ++children;
        }
    }
    if (p == -1 && children > 1)
        is_articulation[v] = true;
}

void find_bridges_and_articulations() {
    timer = 0;
    tin.assign(n, -1);
    low.assign(n, -1);
    is_articulation.assign(n, false);
    is_bridge.assign(n, false); // can track bridges easily
    for (int i = 0; i < n; ++i) {
        if (tin[i] == -1)
            dfs(i);
    }
}
```
## Complexity
**Time:** $O(V + E)$

**Memory:** $O(V + E)$
