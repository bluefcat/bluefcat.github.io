---
layout: post
title: "fast fourier transform algorithm"
tags: Cpp Algorithm Template
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
