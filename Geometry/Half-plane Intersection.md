## Template
```cpp
struct Line {
    Point p, v; // Line passes through p, in direction v
    double ang;
    Line() {}
    Line(Point p, Point v) : p(p), v(v) { ang = atan2(v.y, v.x); }
    bool operator<(const Line& o) const { return ang < o.ang; }
};

bool on_left(Line l, Point p) {
    return cross(l.v, p - l.p) > EPS;
}

Point get_intersection(Line a, Line b) {
    double t = cross(b.p - a.p, b.v) / cross(a.v, b.v);
    return a.p + a.v * t;
}

// Returns the vertices of the convex polygon formed by the intersection
vector<Point> halfplane_intersection(vector<Line> h) {
    sort(h.begin(), h.end());
    deque<Line> q;
    deque<Point> p;
    q.push_back(h[0]);
    for (int i = 1; i < h.size(); i++) {
        while (p.size() >= 1 && !on_left(h[i], p.back())) p.pop_back(), q.pop_back();
        while (p.size() >= 1 && !on_left(h[i], p.front())) p.pop_front(), q.pop_front();
        if (abs(cross(q.back().v, h[i].v)) < EPS) {
            if (on_left(q.back(), h[i].p)) q.back() = h[i];
        } else {
            q.push_back(h[i]);
            p.push_back(get_intersection(q[q.size()-2], q.back()));
        }
    }
    while (p.size() >= 1 && !on_left(q.front(), p.back())) p.pop_back(), q.pop_back();
    if (q.size() <= 2) return vector<Point>();
    p.push_back(get_intersection(q.back(), q.front()));
    return vector<Point>(p.begin(), p.end());
}
```
## Complexity
**Time:** $O(N \log N)$

**Memory:** $O(N)$
