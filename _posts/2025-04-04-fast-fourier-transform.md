---
layout: post
title: "fast fourier transform algorithm"
tags: Template Cpp Algorithm
---

## Implementation
```cpp

#include <complex>
#include <vector>

using complex = std::complex<double>;
using std::vector;
using lint = long long;

constexpr double pi = 3.14159265358979;

void fft(vector<complex>& f, bool inv){
	int n = f.size();
	if(n == 1) return;

	for(int i = 1, j = 0; i < n; i ++){
		int bit = n >> 1;

		while(j >= bit){
			j -= bit;
			bit >>= 1;
		}

		j += bit;
		if(i < j) std::swap(f[i], f[j]);
	}

	for(int k = 1; k < n; k <<= 1){
		double angle = inv?pi/k:-pi/k;
		complex w{cos(angle), sin(angle)};

		for(int i = 0; i < n; i += (k << 1)){
			complex wi{1, 0};

			for(int j = 0; j < k; j ++){
				complex even = f[i+j];
				complex odd = f[i+j+k];
				f[i+j] = even + wi * odd;
				f[i+j+k] = even - wi * odd;
				wi *= w;
			}
		}
	}
	if(inv)
		//M^-1 = 1/n * (M that w has negative angle)
		for(int i = 0; i < n; i++) f[i] /= n;
}
```

```cpp
#include <vector>

using std::vector;
using lint = long long;

//3'221'225'473
//const lint A = 3, B = 30, P = (A << B) | 1, R = 5; 
//2'281'701'377
//const lint A = 17, B = 27, P = (A << B) | 1, R = 3; 
//2'013'265'921
//const lint A = 15, B = 27, P = (A << B) | 1, R = 31; 
//469'762'049
//const lint A = 7, B = 26, P = (A << B) | 1, R = 3; 
//998'244'353
const lint A = 119, B = 23, P = (A << B) | 1, R = 3;

lint pow(lint x, lint e, lint mod = P){
	lint result = 1;
	while(e){
		if(e & 1) result = (result * x) % mod;
		x = (x * x) % mod;
		e >>= 1;
	}
	return result;
}

void ntt(vector<lint>& f, bool inv = false){
	lint n = f.size();
	if(n == 1) return;

	for(lint i = 1, j = 0; i < n; i ++){
		lint bit = n >> 1;
		for(;j&bit; bit >>= 1) j^=bit;
		j ^= bit;

		if(i < j) std::swap(f[i], f[j]);
	}

	for(lint k = 1; k < n; k <<= 1){
		lint w = pow(inv?pow(R,P-2):R, P / k >> 1);

		for(lint i = 0; i < n; i += (k << 1)){
			lint wi=1;

			for(lint j = 0; j < k; j ++){
				lint even = f[i+j];
				lint odd = (f[i+j+k] * wi) % P;
				f[i+j] = (even + odd) % P;
				f[i+j+k] = (even - odd + P) % P; 
				wi = (wi*w) % P;
			}
		}
	}
	if(inv){
		lint ip = pow(n, P-2);
		//M^-1 = 1/n * (M that w has negative angle)
		for(lint& x: f)
			x = (x * ip) % P;
	}
}
```
