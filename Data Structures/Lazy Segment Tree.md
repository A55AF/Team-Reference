## Template
```cpp
struct Node {
    ll val;
};
Node merge(Node a, Node b) {
    return {a.val + b.val};
}
Node Neutral = {};
struct lazy_segmentTree {
private:
    void propagate(ll node, ll lx, ll rx) {
        if (lazy[node] == 0)
            return;
        if (lx != rx) {
            lazy[left(node)] += lazy[node];
            lazy[right(node)] += lazy[node];
        } else {
            a[lx] += lazy[node];
            tree[node] = {a[lx]};
            lazy[node] = 0;
            return;
        }
        tree[node].val += lazy[node] * (rx - lx + 1);
        lazy[node] = 0;
    }
    ll left(ll x) {
        return 2 * x + 1;
    }
    ll right(ll x) {
        return 2 * x + 2;
    }
    ll mid(ll l, ll r) {
        return l + (r - l) / 2;
    }
    vector<Node> tree;
    vector<ll> lazy;
    vector<ll> a;
    void build(ll node, ll lx, ll rx) {
        if (lx == rx) {
            tree[node] = {a[lx]};
            return;
        }
        build(left(node), lx, mid(lx, rx));
        build(right(node), mid(lx, rx) + 1, rx);
        tree[node] = merge(tree[left(node)], tree[right(node)]);
    }
    void update(ll node, ll lx, ll rx, ll l, ll r, ll x) {
        propagate(node, lx, rx);
        if (lx >= l && rx <= r) {
            lazy[node] += x;
            propagate(node, lx, rx);
            return;
        }
        if (lx > r || rx < l) {
            return;
        }
        update(left(node), lx, mid(lx, rx), l, r, x);
        update(right(node), mid(lx, rx) + 1, rx, l, r, x);
        tree[node] = merge(tree[left(node)], tree[right(node)]);
    }
    Node query(ll node, ll lx, ll rx, ll l, ll r) {
        propagate(node, lx, rx);
        if (lx >= l && rx <= r) {
            return tree[node];
        }
        if (lx > r || rx < l) {
            return Neutral;
        }
        return merge(query(left(node), lx, mid(lx, rx), l, r), query(right(node), mid(lx, rx) + 1, rx, l, r));
    }
    ll n;
public:
    lazy_segmentTree(vector<ll> &vec) {
        n = vec.size();
        tree.resize(4 * n);
        lazy.resize(4 * n, 0);
        a = vec;
        build(0, 0, n - 1);
    }
    void update(ll l, ll r, ll x) {
        update(0, 0, n - 1, l, r, x);
    }
    Node query(ll l, ll r) {
        return query(0, 0, n - 1, l, r);
    }
};
```