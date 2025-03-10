---
layout: post
title:  "lvalue, rvalue and reference"
tags: Cpp Modern_Cpp
---

## l-value and r-value
### Definition
- l-value 
    - It refers to a memory location and is a value that can be referenced using the & operator.
- r-value
    - It is a value that is not an l-value.  

### Examples
```cpp

int x = 27; //x is l-value, 27 is r-value
int y = 42; //y is l-value, 42 is r-value

x = y; // OK
y = x; // OK

int z = x * y; // z is l-value, x * y is r-value
x * y = 42;    // Error! x * y is r-value

//l-values
int x;
int* p1 = &x;
int& foo(); //foo() = 27;
int* p2 = foo();

//r-values
int bar(); // int x = bar();
int* p3 = bar(); //Error! bar() is r-value
```

## Reference
### Definition
reference is an alias for an object

### l-value reference
```cpp

(type name)& (identifier) = (l-value expression);
```
Exceptionally, an l-value reference with the **const** keyword can refer to an r-value

### l-value reference examples
```cpp

int a = 3;
int& b = a;

int& c = 3; //Error! 3 is r-value
const int& d = 3; 
```

### r-value reference examples
Added since C++11
```cpp

(type name)&& (identifier) = (r-value expression);
```

### r-value reference examples
```cpp

int sum(int a, int b){
    return a + b;
}

int&& val = sum(5, 4); //val is x-value (eXpiring value)

```

### move semantics
```cpp

class X{
    X();
    X(const X&);
    X(X&&);
    X& operator=(const X&); //Assignment operator
    X& operator=(X&&);      //Move assignment operator
};

X a;    //Called X()
X b(a); //Called X(const X&)
X c(std::move(b)); //Called X(X&&)

c = a; //Called opeartor=(const X&)

X d;
d = std::move(c); //Called operator=(X&&), now c can no longer be used

```





