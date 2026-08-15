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

## Template (Yasser)
```cpp
struct Node {
    ll gd;
};
Node merge(Node a, Node b) { return {gcd(a.gd, b.gd)}; }
Node neutral = {};
template <typename T> class segmentTree {
private:
    ll left(ll a) { return 2 * a + 1; }
    ll right(ll a) { return 2 * a + 2; }
    ll mid(ll a, ll b) { return a + (b - a) / 2; }
    ll n;
    vector<T> arr;
    vector<Node> tree;
    void build(ll node, ll lx, ll rx) {
        if (lx == rx) {
            tree[node] = {arr[lx]};
            return;
        }
        ll m = mid(lx, rx);
        build(left(node), lx, m);
        build(right(node), m + 1, rx);
        tree[node] = merge(tree[left(node)], tree[right(node)]);
    }
    void update(ll node, ll lx, ll rx, ll idx, T val) {
        if (lx == rx) {
            arr[idx] = val;
            tree[node] = {arr[lx]};
            return;
        }
        ll m = mid(lx, rx);
        if (idx <= m) {
            update(left(node), lx, m, idx, val);
        } else {
            update(right(node), m + 1, rx, idx, val);
        }
        tree[node] = merge(tree[left(node)], tree[right(node)]);
    }
    Node query(ll node, ll lx, ll rx, ll l, ll r) {
        if (lx >= l && rx <= r) {
            return tree[node];
        }
        if (lx > r || rx < l) {
            return neutral;
        }
        ll m = mid(lx, rx);
        return merge(query(left(node), lx, m, l, r),
                     query(right(node), m + 1, rx, l, r));
    }

public:
    segmentTree(vector<T> &a) {
        arr = a;
        n = a.size();
        tree.resize(4 * n);
        build(0, 0, n - 1);
    }
    void update(ll idx, T val) { update(0, 0, n - 1, idx, val); }
    Node query(ll l, ll r) { return query(0, 0, n - 1, l, r); }
};
```
## Complexity
**Time:** 
 - **build:** $O(N)$
 - **query:** $O(logN)$
 - **update:** $O(logN)$

**Memory:** $O(N)$