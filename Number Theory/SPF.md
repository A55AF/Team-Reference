```cpp
int spf[N];
void SPF() {
    for(int i = 0; i < N; i++)
        spf[i] = i;

    for(int i = 2; i * i < N; i++) {
        if(spf[i] != i) continue;
        for(int j = i * i; j < N; j += i)
            spf[j] = min(spf[j], i);
    }
}
```

## Complexity
**Time:** $O(Nlog(log(N)))$

**Memory:** $O(N)$
