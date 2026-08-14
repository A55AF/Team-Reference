## Template
```cpp
struct LCA 
{ 
    vector<vector<ll>> par; 
    vector<ll> in; 
    vector<ll> out; 
    vector<ll> dist; 
    ll timer = 0; 
    ll root; 
    ll n; 
    void bfs(vector<vector<ll>> &adj) 
    { 
        dist[root] = 0; 
        queue<ll> q; 
        q.push(root); 
        while (!q.empty()) 
        { 
            ll node = q.front(); 
            q.pop(); 
            for (auto child : adj[node]) 
            { 
                if (dist[child] == INF) 
                { 
                    q.push(child); 
                    dist[child] = dist[node] + 1; 
                } 
            } 
        } 
    } 
    void dfs(ll node, ll parent, vector<vector<ll>> &adj) 
    { 
        in[node] = timer++; 
        par[node][0] = parent; 
        for (int j = 1; j < LG; j++) 
        { 
            par[node][j] = par[par[node][j - 1]][j - 1]; 
        } 
        for (auto child : adj[node]) 
        { 
            if (parent == child) 
                continue; 
 
            dfs(child, node, adj); 
        } 
        out[node] = timer++; 
    } 
    ll getKth(ll node, ll k) 
    { 
        for (ll j = 0; j < LG; j++) 
        { 
            if ((k >> j) & 1) 
            { 
                node = par[node][j]; 
            } 
        } 
        return node; 
    } 
    bool isPar(ll p, ll v) 
    { 
        return in[p] <= in[v] && out[p] >= out[v]; 
    } 
    LCA(ll rooti, ll ni, vector<vector<ll>> &adj) 
    { 
        root = rooti; 
        n = ni; 
        par.resize(n + 1, vector<ll>(LG)); 
        in.resize(n + 1); 
        out.resize(n + 1); 
        dist.resize(n + 1, INF); 
 
        dfs(root, root, adj); 
        bfs(adj); 
    } 
    ll getLCA(ll u, ll v) 
    { 
        if (isPar(u, v)) 
        { 
            return u; 
        } 
        for (ll i = LG - 1; i >= 0; i--) 
        { 
            if (!isPar(par[u][i], v)) 
            { 
                u = par[u][i]; 
            } 
        } 
        return par[u][0]; 
    } 
};
```