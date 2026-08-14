# Summation Formulas

## Sum of Powers

The sum of powers of $n$ from $n^1$ to $n^k$ is:
$$
∑_{i=1}^k n^i = \frac{n(n^k-1)}{n-1}
$$
Where:
- $n$ is the base number
- $k$ is the highest power
- $i$ is the summation index

## Geometric Series

Finite geometric series formula:
$$
\sum_{i=0}^{n} ar^i = a\frac{1-r^{n+1}}{1-r}
$$

Infinite geometric series (where $|r| < 1$):
$$
\sum_{i=0}^{\infty} ar^i = \frac{a}{1-r}
$$
Where:
- $a$ is the first term in the series
- $r$ is the common ratio between consecutive terms
- $n$ is the number of terms (for finite series)
## Binomial Coefficient Sum

The sum of specific binomial coefficients follows:
$$
(n)C1+(n-1)C2+(n-2)C3+\ldots = \text{fibonacci}[n]-1
$$
Where $fibonacci[0] = 1$.
# Number Theory

## Divisor Properties

Number of divisors/factors of a number $n$:
$$
\text{Divisors}(n) = \prod_{i=1}^{\text{primes}} (\text{power} + 1)
$$
Where $power$ is the exponent of each prime in the prime factorization of $n$.

Sum of all factors:
$$
\text{Sum of factors}(n) = \prod_{i=1}^{\text{primes}} \frac{\text{prime}^{\text{cnt}+1} - 1}{\text{prime} - 1}
$$
Where $prime$ is each prime factor and $cnt$ is its exponent in the prime factorization.

Product of all factors:
$$
\text{Product of factors}(n) = n^{\text{number of div}/2}
$$
Where $\text{number of div}$ is the total number of divisors of $n$.

## Floor Function Properties

The maximum number of different values of $floor(\frac{n}{i})$ where $(1 < i < n)$ is:
$$
2 \cdot \sqrt{n}
$$
## GCD Reduction Formula
$$
gcd(x, y) = gcd(x - y, y) = gcd(abs(x - y), y)
$$
applies for more than two arguments.

# Modular Arithmetic

## Modular Addition Properties

$(A_i + K) \mod M$  is equivalent to:
$$
(A_i + K) \mod M = (A_i + \gcd(K,M)) \mod M
$$
Where:
- $A_i$ is the original value
- $K$ is the value being added
- $M$ is the modulus
- $gcd$ is the greatest common divisor

# Euclidean GCD Formula
$$
gcd(a, b) = gcd(a \ \text{mod} \ b, b) = gcd(a, b \ \text{mod} \ a)
$$
