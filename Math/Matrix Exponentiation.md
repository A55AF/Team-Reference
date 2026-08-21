## Template
```cpp
const long long MOD = 1e9 + 7;

struct Matrix {
    vector<vector<long long>> mat;
    int r, c;
    Matrix(int r, int c) : r(r), c(c) {
        mat.assign(r, vector<long long>(c, 0));
    }
    Matrix operator*(const Matrix& other) const {
        Matrix res(r, other.c);
        for (int i = 0; i < r; ++i) {
            for (int k = 0; k < c; ++k) {
                for (int j = 0; j < other.c; ++j) {
                    res.mat[i][j] = (res.mat[i][j] + mat[i][k] * other.mat[k][j]) % MOD;
                }
            }
        }
        return res;
    }
};

Matrix power(Matrix a, long long p) {
    Matrix res(a.r, a.c);
    for (int i = 0; i < a.r; ++i) res.mat[i][i] = 1; // Identity matrix
    while (p > 0) {
        if (p & 1) res = res * a;
        a = a * a;
        p >>= 1;
    }
    return res;
}
```
## Complexity
**Time:** $O(K^3 \log N)$

**Memory:** $O(K^2)$
