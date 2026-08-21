## Template
```cpp
vector<vector<int>> adj;
vector<int> parent, depth, heavy, head, pos;
int cur_pos;

int dfs(int v) {
    int size = 1;
    int max_sub_sz = 0;
    for (int c : adj[v]) {
        if (c != parent[v]) {
            parent[c] = v;
            depth[c] = depth[v] + 1;
            int sub_sz = dfs(c);
            size += sub_sz;
            if (sub_sz > max_sub_sz) {
                max_sub_sz = sub_sz;
                heavy[v] = c;
            }
        }
    }
    return size;
}

void decompose(int v, int h) {
    head[v] = h;
    pos[v] = cur_pos++;
    if (heavy[v] != -1)
        decompose(heavy[v], h);
    for (int c : adj[v]) {
        if (c != parent[v] && c != heavy[v]) {
            decompose(c, c);
        }
    }
}

void init_hld(int n, int root = 0) {
    parent.assign(n, -1);
    depth.assign(n, 0);
    heavy.assign(n, -1);
    head.assign(n, 0);
    pos.assign(n, 0);
    cur_pos = 0;
    dfs(root);
    decompose(root, root);
}

// Example query logic for paths
int query_path(int a, int b) {
    int res = 0; // identity
    for (; head[a] != head[b]; b = parent[head[b]]) {
        if (depth[head[a]] > depth[head[b]]) swap(a, b);
        // res = merge(res, segment_tree_query(pos[head[b]], pos[b]));
    }
    if (depth[a] > depth[b]) swap(a, b);
    // res = merge(res, segment_tree_query(pos[a], pos[b])); // or pos[a]+1 for edge queries
    return res;
}
```
## Complexity
**Time:** 
 - **build:** $O(N)$
 - **query/update:** $O(\log^2 N)$

**Memory:** $O(N)$
