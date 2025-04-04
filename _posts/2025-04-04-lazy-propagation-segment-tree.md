---
layout: post
title: "lazy propagation segment tree"
tags: Template Cpp DataStructure
---

## Implementation
```cpp

using lint = long long;

constexpr int N = 10;
constexpr int get_left(int idx){
	return (idx << 1) + 1;
}
constexpr int get_right(int idx){
	return (idx << 1) + 2;
}

lint arr[N];
lint tree[4*N+4][2];

void propagate(int l, int r, int idx){
	if(tree[idx][1] == 0) return;
	if(l != r){
		//update query
		//tree[get_left(idx)][1] += tree[idx][1];
		//tree[get_right(idx)][1] += tree[idx][1];
	}
	//tree[idx][0] += tree[idx][1];
	tree[idx][1] = 0;
}

lint update(int l, int r, int s, int e, int v, int idx){
	propagate(l, r, idx);
	if(r < s || e < l) return tree[idx][0];
	if(s <= l && r <= e){
		//tree[idx][1] = v;
		propagate(l, r, idx);
		return tree[idx][0];
	}
	int m = (l + r) >> 1;
	return tree[idx][0] = update(l, m, s, e, v, get_left(idx)) + update(m+1, r, s, e, v, get_right(idx));
}

lint query(int l, int r, int s, int e, int idx){
	propagate(l, r, idx);
	if(r < s || e < l) return 0;
	if(s <= l && r <= e) return tree[idx][0];
	int m = (l + r) >> 1;
	return query(l, m, s, e, get_left(idx)) + query(m+1, r, s, e, get_right(idx));
}

```
