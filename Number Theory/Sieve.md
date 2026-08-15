## Template
```cpp
bool not_primes[N];
void sieve() {
    for (int i = 2; i * i < N; i++) {
        if (!not_primes[i]) {
            for (int j = i * i; j < N; j += i) {
                not_primes[j] = true;
            }
        }
    }
}
```

## Complexity
**Time:** $O(Nlog(log(N)))$

**Memory:** $O(N)$
