## Template
```cpp
// Returns the maximum distance squared between any two points in a convex polygon
long long rotating_calipers(const vector<Point>& p) {
    int n = p.size();
    if (n <= 1) return 0;
    if (n == 2) return dist_sq(p[0], p[1]); // Assuming dist_sq is defined

    int k = 1;
    long long max_d = 0;
    
    // Find the furthest point from the segment p[0]-p[n-1]
    while (abs(cross(p[n - 1], p[0], p[(k + 1) % n])) > abs(cross(p[n - 1], p[0], p[k]))) {
        k++;
    }

    int j = k;
    for (int i = 0; i < k; i++) {
        max_d = max(max_d, dist_sq(p[i], p[j]));
        while (j < n && abs(cross(p[i], p[(i + 1) % n], p[(j + 1) % n])) > abs(cross(p[i], p[(i + 1) % n], p[j]))) {
            max_d = max(max_d, dist_sq(p[i], p[(j + 1) % n]));
            j++;
        }
        max_d = max(max_d, dist_sq(p[i], p[j]));
    }
    return max_d;
}
// Note: dist_sq(a, b) = (a.x - b.x)^2 + (a.y - b.y)^2
// cross(a, b, c) = cross product of vectors AB and AC
```
## Complexity
**Time:** $O(N)$

**Memory:** $O(N)$
