### Max Subarray XOR
```cpp
dp[l][r] = max({ pref[r + 1] ^ pref[l], dp[l + 1][r], dp[l][r - 1] })
```

### Longest Palindromic Subsequence (LPS)
```cpp
dp[l][r] = (S[l] == S[r]) ? (2 + dp[l + 1][r - 1]) : max(dp[l + 1][r], dp[l][r - 1])
```

### Stone Game Pick (Max Advantage)
```cpp
dp[l][r] = max(V[l] - dp[l + 1][r], V[r] - dp[l][r - 1])
```

### Parentheses Match (Min Deletions)
```cpp
dp[l][r] = (S[l] matches S[r]) ? dp[l + 1][r - 1] : (1 + min(dp[l + 1][r], dp[l][r - 1]))
```