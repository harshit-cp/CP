# 🔢 Math & Geometry

## Overview
Mathematical concepts and geometric algorithms.

## Key Concepts

### Number Theory
- GCD/LCM (Euclidean algorithm)
- Prime numbers (Sieve of Eratosthenes)
- Modular arithmetic
- Fast exponentiation

### Geometry
- Distance calculations
- Line intersections
- Convex hull
- Area calculations

## Templates

```cpp
// GCD
int gcd(int a, int b) {
    return b == 0 ? a : gcd(b, a % b);
}

// LCM
int lcm(int a, int b) {
    return a / gcd(a, b) * b;
}

// Sieve of Eratosthenes
vector<bool> sieve(int n) {
    vector<bool> isPrime(n + 1, true);
    isPrime[0] = isPrime[1] = false;
    for (int i = 2; i * i <= n; i++) {
        if (isPrime[i]) {
            for (int j = i * i; j <= n; j += i) {
                isPrime[j] = false;
            }
        }
    }
    return isPrime;
}

// Fast Exponentiation
long long power(long long base, long long exp, long long mod) {
    long long result = 1;
    base %= mod;
    while (exp > 0) {
        if (exp & 1) result = result * base % mod;
        base = base * base % mod;
        exp >>= 1;
    }
    return result;
}

// Modular Inverse (when mod is prime)
long long modInverse(long long a, long long mod) {
    return power(a, mod - 2, mod);
}

// Check if prime
bool isPrime(int n) {
    if (n < 2) return false;
    if (n == 2) return true;
    if (n % 2 == 0) return false;
    for (int i = 3; i * i <= n; i += 2) {
        if (n % i == 0) return false;
    }
    return true;
}
```

## Problems Solved

| # | Problem | Difficulty | Pattern | Status |
|---|---------|------------|---------|--------|
| | | | | |

## Quick Tips
- Large numbers → Use modular arithmetic
- "Number of divisors" → Prime factorization
- Geometry problems → Watch for precision issues
