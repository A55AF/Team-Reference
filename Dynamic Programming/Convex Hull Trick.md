## Template
```cpp
// Li Chao Tree (Minimization)
// Handles lines of the form: y = m*x + c
struct Line {
    long long m, c;
    long long eval(long long x) { return m * x + c; }
};

struct LiChao {
    struct Node {
        Line line = {0, (long long)1e18}; // Identity for minimization
        int l = -1, r = -1;
    };
    vector<Node> tree;
    long long MIN_X, MAX_X;

    LiChao(long long min_x, long long max_x) : MIN_X(min_x), MAX_X(max_x) {
        tree.push_back(Node());
    }

    void add_line(int node, long long l, long long r, Line new_line) {
        long long mid = l + (r - l) / 2;
        bool left = new_line.eval(l) < tree[node].line.eval(l);
        bool mid_val = new_line.eval(mid) < tree[node].line.eval(mid);

        if (mid_val) swap(tree[node].line, new_line);
        if (l == r) return;

        if (left != mid_val) {
            if (tree[node].l == -1) { tree[node].l = tree.size(); tree.push_back(Node()); }
            add_line(tree[node].l, l, mid, new_line);
        } else {
            if (tree[node].r == -1) { tree[node].r = tree.size(); tree.push_back(Node()); }
            add_line(tree[node].r, mid + 1, r, new_line);
        }
    }
    void add(long long m, long long c) { add_line(0, MIN_X, MAX_X, {m, c}); }

    long long query(int node, long long l, long long r, long long x) {
        if (node == -1) return 1e18;
        long long mid = l + (r - l) / 2;
        long long res = tree[node].line.eval(x);
        if (l == r) return res;
        if (x <= mid) return min(res, query(tree[node].l, l, mid, x));
        else return min(res, query(tree[node].r, mid + 1, r, x));
    }
    long long get(long long x) { return query(0, MIN_X, MAX_X, x); }
};
```
## Complexity
**Time:** $O(N \log X)$ or $O(N)$ (monotonic)

**Memory:** $O(N)$
