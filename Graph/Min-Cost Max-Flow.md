## Template
```cpp
struct MCMF {
    struct Edge {
        int to;
        long long cap, flow, cost;
        int rev;
    };
    int n;
    vector<vector<Edge>> adj;
    vector<long long> dist, pot;
    vector<int> parent_edge, parent_node;
    const long long INF = 1e18;

    MCMF(int n) : n(n), adj(n), dist(n), pot(n, 0), parent_edge(n), parent_node(n) {}

    void add_edge(int from, int to, long long cap, long long cost) {
        adj[from].push_back({to, cap, 0, cost, (int)adj[to].size()});
        adj[to].push_back({from, 0, 0, -cost, (int)adj[from].size() - 1});
    }

    bool dijkstra(int s, int t) {
        fill(dist.begin(), dist.end(), INF);
        priority_queue<pair<long long, int>, vector<pair<long long, int>>, greater<>> pq;
        dist[s] = 0;
        pq.push({0, s});
        
        while (!pq.empty()) {
            auto [d, u] = pq.top();
            pq.pop();
            
            if (dist[u] < d) continue;
            
            for (int i = 0; i < adj[u].size(); ++i) {
                Edge& e = adj[u][i];
                if (e.cap - e.flow > 0 && dist[e.to] > dist[u] + e.cost + pot[u] - pot[e.to]) {
                    dist[e.to] = dist[u] + e.cost + pot[u] - pot[e.to];
                    parent_node[e.to] = u;
                    parent_edge[e.to] = i;
                    pq.push({dist[e.to], e.to});
                }
            }
        }
        return dist[t] != INF;
    }

    pair<long long, long long> max_flow_min_cost(int s, int t) {
        long long flow = 0, cost = 0;
        // If graph has negative edges initially, run SPFA/Bellman-Ford once to initialize pot array.
        
        while (dijkstra(s, t)) {
            for (int i = 0; i < n; ++i) {
                if (dist[i] != INF) pot[i] += dist[i];
            }
            
            long long pushed = INF;
            int curr = t;
            while (curr != s) {
                int p = parent_node[curr];
                int idx = parent_edge[curr];
                pushed = min(pushed, adj[p][idx].cap - adj[p][idx].flow);
                curr = p;
            }
            
            flow += pushed;
            curr = t;
            while (curr != s) {
                int p = parent_node[curr];
                int idx = parent_edge[curr];
                adj[p][idx].flow += pushed;
                adj[curr][adj[p][idx].rev].flow -= pushed;
                cost += pushed * adj[p][idx].cost;
                curr = p;
            }
        }
        return {flow, cost};
    }
};
```
## Complexity
**Time:** $O(F \cdot E \log V)$ (using Successive Shortest Path with Dijkstra + Potentials)

**Memory:** $O(V + E)$
