## Template
```cpp
struct Trie {
    struct Node {
        static const int MX = 26;
        int children[MX]{};
        int frq = 0;
    };
    vector<Node> trie;

    Trie() {
        trie.emplace_back();
    }

    void insert(const string&s) {
        int idx = 0;
        for ( const char&i : s ) {
            int nxt = i - 'a';

            if ( !trie[idx].children[nxt] ) {
                trie[idx].children[nxt] = trie.size();
                trie.emplace_back();
            }
            idx = trie[idx].children[nxt];
            trie[idx].frq++;
        }
    }

    void remove(const string&s) {
        int idx = 0;
        for (const char&i : s) {
            int nxt = i - 'a';
            idx = trie[idx].children[nxt];
            trie[idx].frq--;
        }
    }

    int query(const string&s) {
        int idx = 0;
        for (const char&i : s) {
            int nxt = i - 'a';
            if (!trie[trie[idx].children[nxt]].frq)
                return 0;
            idx = trie[idx].children[nxt];
        }
        return trie[idx].frq;
    }
};
```

## Complexity
**Time:** $O(N)$ (N: Size of String)

**Memory:** $O(N)$
