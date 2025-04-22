---
layout: post
title: "dancing link with Knuth's X"
tags: Template Cpp Algorithm
---

## Implementation
```cpp
constexpr int N = 501;

struct Node{
	int row;
	int size;
	Node* col{nullptr};
	Node *up{this}, *down{this}, *left{this}, *right{this};	
};

void dlx_cover(Node* head){
	//hide head Node
	/*
	 * [l | A | r] <-> [l | head | r] <-> [l | B | r]
	 * [l | A | r] <--------------------> [l | B | r]
	 */
	head->right->left = head->left;
	head->left->right = head->right;
	
	//circular linked list
	for(Node* it = head->down; it != head; it = it->down){
		for(Node* jt = it->right; jt != it; jt = jt->right){
			jt->down->up = jt->up;
			jt->up->down = jt->down;
			jt->col->size --;
		}
	}
}

void dlx_uncover(Node* head){
    //show head Node
	for(Node* it = head->up; it != head; it = it->up){
		for(Node* jt = it->left; jt != it; jt = jt->left){
			jt->down->up = jt;
			jt->up->down = jt;
			jt->col->size ++;
		}
	}
	
	head->left->right = head;
	head->right->left = head;
}

bool dlx_search(Node* head, int k, std::vector<int>& solution){
	if(head->right == head) return true;

	Node* ptr = nullptr;
	int low = INT_MAX;
	
	for(Node* it = head->right; it != head; it = it->right){
		if(it->size < low){
			if(it->size == 0) return false;
			low = it->size;
			ptr = it;
		}
	}

	dlx_cover(ptr);
	
	for(Node* it = ptr->down; it != ptr; it = it->down){
		solution.push_back(it->row);
		for(Node* jt = it->right; jt != it; jt = jt->right){
			dlx_cover(jt->col);
		}

		if(dlx_search(head, k+1, solution)) return true;

		solution.pop_back();
		for(Node* jt = it->left; jt != it; jt = jt->left){
			dlx_uncover(jt->col);
		}
	}
	dlx_uncover(ptr);
	return false;
}


Node* board_[N];

void init(Node* head){
	Node* cur = head;
	for(int t = 0; t < N; t ++){
        Node* h = new Node{0, 0};
        h->up = h; h->down = h;
        h->left = cur; h->right = cur->right;
        cur->right = h;
        board_[t] = h;
        cur = h;
	}
}

template<typename T>
Node* append(T row, T head){
	Node* result = new Node{row};
	Node* h = board_[head];

	h->size ++;
	Node* it = h;
	for(;it->down != h; it = it->down);
	
	result->up = it; 
	it->down = result;
	result->down = h; 
	h->up = result;
	result->col = h;

	return result;
}

template<typename T, typename... Args>
Node* append(T row, T head, Args... args){
	Node* result = append(row, head);
	Node* next = append(args...);
	
	result->right = next; 
	result->left = next->left;

	next->left->right = result;
	next->left = result;
	return result;
}
```
