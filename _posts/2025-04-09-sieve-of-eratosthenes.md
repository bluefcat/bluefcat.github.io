---
layout: post
title: "sieve of eratosthenes algorithm"
tags: Template Cpp Prime Math Algorithm
---

## Implementation
```cpp

constexpr int N = 10001;
bool prime[N];

void build_sieve(){
	for(int i = 0; i < N; i ++) prime[i] = true;
	int idx = 3;
	int k = 3;
	for(int i = 0; idx < N; i ++){
		if(prime[i]){ //2*i+3
			prime[idx] = false;
			for(int j = idx; N - j > k; j += k){
				prime[j] = false;
			}
		}
		idx += k;
		k += 2;
		idx += k;
	}
}
```
