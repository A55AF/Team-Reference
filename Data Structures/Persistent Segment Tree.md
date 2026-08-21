## Template
```cpp
struct Node {
    int val = 0;
    int left = 0, right = 0;
};

// tree[0] is a dummy null node
vector<Node> tree(1);
vector<int> roots; 

int build(int tl, int tr, const vector<int>& a) {
    int node = tree.size();
    tree.push_back(Node());
    if (tl == tr) {
        tree[node].val = a[tl];
        return node;
    }
    int tm = tl + (tr - tl) / 2;
    tree[node].left = build(tl, tm, a);
    tree[node].right = build(tm + 1, tr, a);
    tree[node].val = tree[tree[node].left].val + tree[tree[node].right].val;
    return node;
}

int update(int prev_node, int tl, int tr, int idx, int val) {
    int node = tree.size();
    tree.push_back(tree[prev_node]); // Copy previous state
    if (tl == tr) {
        tree[node].val = val;
        return node;
    }
    int tm = tl + (tr - tl) / 2;
    if (idx <= tm) {
        tree[node].left = update(tree[prev_node].left, tl, tm, idx, val);
    } else {
        tree[node].right = update(tree[prev_node].right, tm + 1, tr, idx, val);
    }
    tree[node].val = tree[tree[node].left].val + tree[tree[node].right].val;
    return node;
}

int query(int node, int tl, int tr, int l, int r) {
    if (l > tr || r < tl || node == 0) return 0;
    if (l <= tl && tr <= r) return tree[node].val;
    int tm = tl + (tr - tl) / 2;
    return query(tree[node].left, tl, tm, l, r) +
           query(tree[node].right, tm + 1, tr, l, r);
}
```
## Complexity
**Time:** 
 - **build:** $O(N \log N)$
 - **query:** $O(\log N)$
 - **update:** $O(\log N)$

**Memory:** $O(N \log N)$ (or $O(Q \log N)$)
