## Template
```cpp
// Returns the convex hull of the given points in counter-clockwise order
vector<Point> convex_hull(vector<Point> pts) {
    if (pts.size() <= 1) return pts;
    sort(pts.begin(), pts.end()); // Requires operator< to sort by X, then Y
    
    vector<Point> h;
    // Lower hull
    for (const auto& p : pts) {
        while (h.size() >= 2 && ccw(h[h.size()-2], h.back(), p) <= 0)
            h.pop_back();
        h.push_back(p);
    }
    // Upper hull
    for (int i = pts.size() - 2, t = h.size() + 1; i >= 0; i--) {
        while (h.size() >= t && ccw(h[h.size()-2], h.back(), pts[i]) <= 0)
            h.pop_back();
        h.push_back(pts[i]);
    }
    h.pop_back(); // Remove duplicate starting point
    return h;
}
```
## Complexity
**Time:** $O(N \log N)$

**Memory:** $O(N)$
