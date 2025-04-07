---
layout: post
title: "lowest common ancestor algorithm"
tags: Template Cpp Algorithm
---

## Implementation
```cpp

constexpr int N = 50001;
constexpr int LN = 19;

//T is unordered_map<int, vector<int>>
template<typename T>
void build_depth(
	T& graph, int* depth, int table[][N], int root, int idx	
){
	for(auto n: graph[idx]){
		if(root == n || depth[n] != 0) continue;	
		table[0][n] = idx;
		depth[n] = depth[idx]+1;
		build_depth(graph, depth, table, root, n);
	}
}

int LCA(int* depth, int table[][N], int u, int v){
	if(depth[u] > depth[v]){
		int tmp = u; u = v; v = tmp;
	}
	
	int diff = depth[v] - depth[u];
	
	for(int i = LN-1; i >= 0; i --){
		if((diff & (1 << i)) == (1 << i)) v = table[i][v];
	}

	if(u != v){
		for(int i = LN-1; i >= 0; i --){
			if(table[i][u] != 0 && table[i][u] != table[i][v]){
				u = table[i][u];
				v = table[i][v];
			}
		}
		u = table[0][u];
	}
	return u;
}
```
