## Template
```cpp
// Returns a vector d, where d[i] is the length of the palindrome centered at i
// Includes dummy characters '#' to handle even length palindromes.
// E.g., "aba" -> "#a#b#a#"
vector<int> manacher(string s) {
    string t = "#";
    for (char c : s) { t += c; t += '#'; }
    int n = t.size();
    vector<int> d(n);
    for (int i = 0, l = 0, r = -1; i < n; i++) {
        int k = (i > r) ? 1 : min(d[l + r - i], r - i + 1);
        while (0 <= i - k && i + k < n && t[i - k] == t[i + k]) {
            k++;
        }
        d[i] = k--;
        if (i + k > r) {
            l = i - k;
            r = i + k;
        }
    }
    return d; // Actual length of palindrome centered at i is d[i] - 1
}
```
## Complexity
**Time:** $O(N)$

**Memory:** $O(N)$
