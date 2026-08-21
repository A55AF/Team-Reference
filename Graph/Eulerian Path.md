## Template
```cpp
// Hierholzer's Algorithm (Directed Graph Example)
// For undirected, track used edges using an array or map
vector<vector<int>> adj;
vector<int> in_deg, out_deg;

vector<int> find_eulerian_path(int n) {
    int start_node = -1, end_node = -1;
    for (int i = 0; i < n; ++i) {
        if (out_deg[i] - in_deg[i] == 1) {
            if (start_node != -1) return {}; // No path
            start_node = i;
        } else if (in_deg[i] - out_deg[i] == 1) {
            if (end_node != -1) return {}; // No path
            end_node = i;
        } else if (in_deg[i] != out_deg[i]) {
            return {}; // No path
        }
    }
    if (start_node == -1) {
        for (int i = 0; i < n; ++i) if (out_deg[i] > 0) { start_node = i; break; }
    }
    if (start_node == -1) return {}; // Empty graph

    vector<int> path;
    vector<int> edge_idx(n, 0); // Tracks next unvisited edge
    stack<int> st;
    st.push(start_node);

    while (!st.empty()) {
        int u = st.top();
        if (edge_idx[u] < adj[u].size()) {
            st.push(adj[u][edge_idx[u]++]);
        } else {
            path.push_back(u);
            st.pop();
        }
    }
    reverse(path.begin(), path.end());
    // Verify all edges were used (graph is weakly connected)
    // if (path.size() != total_edges + 1) return {};
    return path;
}
```
## Complexity
**Time:** $O(V + E)$

**Memory:** $O(V + E)$
