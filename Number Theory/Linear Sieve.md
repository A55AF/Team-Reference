## Template
```cpp
const int MAXN = 1e7 + 5;
vector<int> primes;
int spf[MAXN]; // Smallest Prime Factor
int phi[MAXN]; // Euler's Totient Function
int mobius[MAXN]; 

void sieve() {
    phi[1] = 1;
    mobius[1] = 1;
    
    for (int i = 2; i < MAXN; i++) {
        if (spf[i] == 0) {
            spf[i] = i;
            primes.push_back(i);
            phi[i] = i - 1;
            mobius[i] = -1;
        }
        for (int j = 0; j < primes.size() && i * primes[j] < MAXN && primes[j] <= spf[i]; j++) {
            spf[i * primes[j]] = primes[j];
            if (i % primes[j] == 0) {
                phi[i * primes[j]] = phi[i] * primes[j];
                mobius[i * primes[j]] = 0;
            } else {
                phi[i * primes[j]] = phi[i] * (primes[j] - 1);
                mobius[i * primes[j]] = mobius[i] * (-1);
            }
        }
    }
}
```
## Complexity
**Time:** $O(N)$

**Memory:** $O(N)$
