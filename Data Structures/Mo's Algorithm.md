## Template
```cpp
const int BLOCK = 320; // ~sqrt(N)

struct Query {
    int l, r, id;
    bool operator<(const Query& other) const {
        int b1 = l / BLOCK, b2 = other.l / BLOCK;
        if (b1 != b2) return b1 < b2;
        return (b1 & 1) ? r < other.r : r > other.r; // Even/Odd optimization
    }
};

void add(int idx) {
    // Add logic here
}

void remove(int idx) {
    // Remove logic here
}

void solve(vector<Query>& queries) {
    sort(queries.begin(), queries.end());
    int L = 0, R = -1; // Current range
    
    vector<int> ans(queries.size());
    for (auto& q : queries) {
        while (L > q.l) add(--L);
        while (R < q.r) add(++R);
        while (L < q.l) remove(L++);
        while (R > q.r) remove(R--);
        
        // ans[q.id] = current_answer;
    }
}
```
## Complexity
**Time:** $O(N \sqrt{Q})$ or $O(N^{5/3})$ with updates

**Memory:** $O(N)$
