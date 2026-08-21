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
    Point operator/(double c) const { return Point(x / c, y / c); }
    bool operator<(const Point& p) const {
        return x < p.x - EPS || (abs(x - p.x) < EPS && y < p.y - EPS);
    }
    bool operator==(const Point& p) const {
        return abs(x - p.x) < EPS && abs(y - p.y) < EPS;
    }
};

double cross(Point a, Point b) { return a.x * b.y - a.y * b.x; }
double dot(Point a, Point b) { return a.x * b.x + a.y * b.y; }
double length(Point a) { return hypot(a.x, a.y); }
double dist(Point a, Point b) { return length(a - b); }

// Returns positive if c is strictly left of directed line ab
double ccw(Point a, Point b, Point c) {
    return cross(b - a, c - a);
}

// Distance from point P to line AB
double distToLine(Point p, Point a, Point b, Point &c) {
    // c returns the closest point on the line AB
    double d = dist(a, b);
    if (d < EPS) { c = a; return dist(p, a); }
    double u = dot(p - a, b - a) / (d * d);
    c = a + (b - a) * u;
    return dist(p, c);
}

// Distance from point P to line segment AB
double distToLineSegment(Point p, Point a, Point b, Point &c) {
    double d = dist(a, b);
    if (d < EPS) { c = a; return dist(p, a); }
    double u = dot(p - a, b - a) / (d * d);
    if (u < 0.0) { c = a; return dist(p, a); }
    if (u > 1.0) { c = b; return dist(p, b); }
    c = a + (b - a) * u;
    return dist(p, c);
}

// Line intersection
bool line_intersection(Point a, Point b, Point c, Point d, Point &out) {
    double cp = cross(b - a, d - c);
    if (abs(cp) < EPS) return false; // Parallel
    double t = cross(c - a, d - c) / cp;
    out = a + (b - a) * t;
    return true;
}

// Polygon Area
double polygon_area(const vector<Point>& p) {
    double area = 0.0;
    for (int i = 0; i < (int)p.size(); i++) {
        int j = (i + 1) % p.size();
        area += cross(p[i], p[j]);
    }
    return abs(area) / 2.0;
}

// Point in Polygon (Ray Casting algorithm)
// returns 1 if strictly inside, 0 if on boundary, -1 if outside
int point_in_polygon(Point pt, const vector<Point>& p) {
    bool in = false;
    for (int i = 0, j = p.size() - 1; i < (int)p.size(); j = i++) {
        Point c;
        if (distToLineSegment(pt, p[i], p[j], c) < EPS) return 0;
        if (((p[i].y > pt.y) != (p[j].y > pt.y)) &&
            (pt.x < (p[j].x - p[i].x) * (pt.y - p[i].y) / (p[j].y - p[i].y) + p[i].x)) {
            in = !in;
        }
    }
    return in ? 1 : -1;
}

// Rotate point by theta (in radians) around origin
Point rotate(Point p, double theta) {
    return Point(p.x * cos(theta) - p.y * sin(theta), p.x * sin(theta) + p.y * cos(theta));
}
```
## Complexity
**Time:** $O(1)$ for primitives, $O(N)$ for Polygon Area and Point-in-Polygon

**Memory:** $O(1)$
