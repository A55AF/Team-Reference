## Template
```cpp
struct Node {
    int val = 0;
} skip;
struct SegmentTree {
#define tm ((tl + tr) >> 1)
#define left (node << 1)
#define right (node << 1 | 1)
    vector<Node> tree;
    ll n;

    SegmentTree(int sz, vector<int>&a) {
        n = sz;
        tree.resize(n << 2);
        build(1, 0, n-1, a);
    }

    void build(int node, int tl, int tr, vector<int>&a) {
        if ( tl == tr ) {
            tree[node].val = a[tl];
            return;
        }
        build(left, tl, tm , a);
        build(right, tm + 1, tr, a);
        tree[node] = merge(tree[left], tree[right]);
    }

    Node merge(Node a, Node b) {
        Node ans;
        ans.val = a.val + b.val;
        return ans;
    }

    Node query(int node, int tl, int tr, int l, int r) {
        if ( tl > r || tr < l ) return skip;
        if ( tl >= l && tr <= r ) return tree[node];

        Node one = query(left, tl, tm, l, r);
        Node two = query(right, tm + 1, tr, l, r);
        return merge(one, two);
    }

    int query(int l, int r) {
        return query(1, 0, n-1, l, r).val;
    }

    void update(int node, int idx, int value, int tl, int tr) {
        if ( idx < tl || idx > tr ) return;
        if ( tl == tr ) {
            tree[node].val = value;
            return;
        }
        update(left, idx, value, tl, tm);
        update(right, idx, value, tm + 1, tr);
        tree[node] = merge(tree[left], tree[right]);
    }

    void update(int idx, int value) {
        update(1, idx, value, 0, n-1);
    }

#undef tm
#undef left
#undef right
};
```

## Complexity
**Time:** 
 - **build:** $O(\text{N})$
 - **query:** $O(\log_2\text{N})$
 - **update:** $O(\log_2\text{N})$

**Memory:** $O(\text{N})$