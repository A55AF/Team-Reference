## Template
```cpp
const double EPS = 1e-9;
struct Point {
    double x, y;
    Point() {}
    Point(double x, double y) : x(x), y(y) {}
    Point operator-(const Point& p) const { return Point(x - p.x, y - p.y); }
    Point operator+(const Point& p) const { return Point(x + p.x, y + p.y); }
    Point operator*(double c) const { return Point(x * c, y * c); }
    bool operator<(const Point& p) const {
        return x < p.x - EPS || (abs(x - p.x) < EPS && y < p.y - EPS);
    }
};

double cross(Point a, Point b) { return a.x * b.y - a.y * b.x; }
double dot(Point a, Point b) { return a.x * b.x + a.y * b.y; }

// Returns positive if c is strictly left of directed line ab
double ccw(Point a, Point b, Point c) {
    return cross(b - a, c - a);
}

// Line intersection
bool line_intersection(Point a, Point b, Point c, Point d, Point &out) {
    double cp = cross(b - a, d - c);
    if (abs(cp) < EPS) return false; // Parallel
    double t = cross(c - a, d - c) / cp;
    out = a + (b - a) * t;
    return true;
}
```
## Complexity
**Time:** $O(1)$ or $O(N)$

**Memory:** $O(1)$
