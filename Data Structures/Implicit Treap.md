## Template
```cpp
mt19937 rng(chrono::steady_clock::now().time_since_epoch().count());

struct Node {
    int val, prior, sz;
    int lazy; // example for lazy propagation
    Node *l, *r;
    Node(int v) : val(v), prior(rng()), sz(1), lazy(0), l(nullptr), r(nullptr) {}
};
typedef Node* pnode;

int sz(pnode t) { return t ? t->sz : 0; }

void push(pnode t) {
    if (t && t->lazy) {
        // apply lazy
        // if (t->l) t->l->lazy += t->lazy;
        // if (t->r) t->r->lazy += t->lazy;
        t->lazy = 0;
    }
}

void upd(pnode t) {
    if (t) {
        t->sz = 1 + sz(t->l) + sz(t->r);
        // update other values (sum, min, max, etc)
    }
}

void split(pnode t, pnode &l, pnode &r, int k) { // split first k elements to l
    if (!t) return void(l = r = nullptr);
    push(t);
    int cur_sz = sz(t->l) + 1;
    if (k >= cur_sz) {
        split(t->r, t->r, r, k - cur_sz);
        l = t;
    } else {
        split(t->l, l, t->l, k);
        r = t;
    }
    upd(t);
}

void merge(pnode &t, pnode l, pnode r) {
    push(l); push(r);
    if (!l || !r) t = l ? l : r;
    else if (l->prior > r->prior) {
        merge(l->r, l->r, r);
        t = l;
    } else {
        merge(r->l, l, r->l);
        t = r;
    }
    upd(t);
}
```
## Complexity
**Time:** $O(\log N)$ per operation (split, merge, insert, delete)

**Memory:** $O(N)$
