## Template
```cpp
vector<vector<int>> adj;
vector<int> subtree_size, parent_centroid;
vector<bool> is_removed;
int n;

void get_subtree_sizes(int node, int parent = -1) {
    subtree_size[node] = 1;
    for (int child : adj[node]) {
        if (child == parent || is_removed[child]) continue;
        get_subtree_sizes(child, node);
        subtree_size[node] += subtree_size[child];
    }
}

int get_centroid(int node, int parent, int total_nodes) {
    for (int child : adj[node]) {
        if (child == parent || is_removed[child]) continue;
        if (subtree_size[child] > total_nodes / 2)
            return get_centroid(child, node, total_nodes);
    }
    return node;
}

void build_centroid_tree(int node = 0, int parent = -1) {
    get_subtree_sizes(node);
    int centroid = get_centroid(node, -1, subtree_size[node]);
    
    parent_centroid[centroid] = parent;
    is_removed[centroid] = true;
    
    // Compute paths passing through centroid here
    
    for (int child : adj[centroid]) {
        if (!is_removed[child]) {
            build_centroid_tree(child, centroid);
        }
    }
}
```
## Complexity
**Time:** 
 - **build:** $O(N \log N)$

**Memory:** $O(N)$
