---
layout: post
title: "sieve of eratosthenes algorithm"
tags: Template Cpp Prime Math Algorithm
---

## Implementation (linear sieve)
```cpp
#include <vector>

using lint = long long;
constexpr int P = 10001;
bool is_prime[P] = { false, };
std::vector<lint> prime;

void build_sieve(){
	for(int i = 2; i < P; i ++) is_prime[i] = true;	
	for(int p = 2; p < P; p ++){
		if(is_prime[p])
			prime.emplace_back(p);
		
		for(auto& q: prime){
			if(p*q >= P) break;
			is_prime[p*q] = false;
			if(p%q == 0) break;
		}
	}
}
```

## Implementation
```cpp

constexpr int P = 10001;
bool prime[P];

void build_sieve(){
	for(int i = 0; i < P; i ++) prime[i] = true;
	int idx = 3;
	int k = 3;
	for(int i = 0; idx < P; i ++){
		if(prime[i]){ //2*i+3
			prime[idx] = false;
			for(int j = idx; N - j > k; j += k){
				prime[j+k] = false;
			}
		}
		idx += k;
		k += 2;
		idx += k;
	}
}
```

## description
1. The square of the smallest prime factor of any composite number 'c' is less than or equal to 'c'
2. Every composite number less than p^2 is divisible by some prime less than p
3. When sieving with p, start eliminating from p^2.
4. To sieve numbers up to m, it suffices to eliminate using primes p such that p^2 <= m

- value(i) = 2\*i + 3
- index(v) = (v - 3)/2
- step(i) = index((k + 2)\*(2\*i + 3)) - index(k\*(2\*i + 3)) = 2\*i + 3
- index(value(i)^2) = 2\*i^2 + 6\*i + 3
