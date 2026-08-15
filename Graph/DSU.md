## Template
```cpp
const int N = 1e5+7;
int comp, par[N], sz[N];
vector<pair<ll,ll>> history;
void init(int n) { // run it in the main first
    comp = n;
    history.clear();
    for(int i = 0; i <= n; i++) {
        par[i] = i;
        sz[i] = 1;
    }
}
ll root(ll x) {
    return x == par[x] ? x : root(par[x]);
}
bool connected(ll x, ll y) {
    return root(x) == root(y);
}
void connect(ll x, ll y) {
    if(connected(x, y)) return;
    x = root(x);
    y = root(y);
    if(sz[x] >= sz[y])
        swap(x, y);
    par[x] = y;
    sz[y] += sz[x];
    comp--;

    history.push_back({x, y});
}
void rollback() {
    if (history.empty()) return;

    auto [x, y] = history.back();
    history.pop_back();

    sz[y] -= sz[x];
    par[x] = x;
    comp++;
}
void rollback_to(int idx) {
    while(history.size() > idx) {
        rollback();
    }
}
```

## Complexity
**Time:** $O(log\ N)$

**Memory:** $O(N)$
