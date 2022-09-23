---
layout: default
title: Extra
parent: Tipps
grand_parent: C++
nav_order: 1
has_children: false
---
## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Initialization a variable using \{ \}
It's generally a good idea to always use `{}`.

Reason:

* The `{}` is the universial usable

```cpp
# This could be both define a function called obj with no input and return a class MyClass
# This could also be initialize a instance of MyClass with default constructer.
MyClass obj(); # Not ok

# This require a copy constructor
MyClass obj = MyClass(); # Not good

# This is unique and clear
MyClass obj{}; # Good
MyClass obj{1, 2};

# This is allowed
int * arr = new int[] {1, 2, 3}; 
```

* `{}` will first try to use the constructor using initializer_list

```cpp
class MyClass
{
  public:
  std::vector<int> a_;
  MyClass(std::initializer_list<int> initializer_list)
  {
    a_ = std::vector<int>(initializer_list.begin(), initializer_list.end());
    printf("Use initializer list constructor.\n");
  }
  MyClass(const int& input_1, const int& input_2, const int& input_3)
  {
    a_.push_back(input_1);
    a_.push_back(input_2);
    a_.push_back(input_3);
    printf("Use normal constructor.\n");
  }
};
MyClass a{1,2,3}; // print: Use initializer list constructor. 
```
---
## constexpr and const in c++14
Before c++11, `const` are used to express 2 meaning:
* Unchangable/read only: a in `void func(const int a);`
* Constant expression: a in `const int a = 1;`

which is confused sometime and reduce the optimization possibility in compiler.

&nbsp;

`constexpr` is introduced in c++11 and optimized in c++14. Compare to const, it has the following meaning:
* More strict constrain
* Only allow constant expression which will be calculated during compiling time.
* Strong optimization inside compiler.

&nbsp;

**Example:**

```cpp
constexpr int x1 = 1; // good: normal initialization

constexpr std::vector<int> x2={1,2,3}; // compile error: initilizer_list constructor not declared as constexpr, need dynamic memory allocation

constexpr std::array<int,3> x3{1,2,3};    // good: std::array doesn't use dynamic memory allocation, it is allocated in stack

constexpr int x5 = x3.size(); // good: x3 is calculated during compiliation, thus size is a constexpr.

std::vector<int> x4;
x4.push_back(1);
constexpr int x6 = x4.size(); // compile error: x4 could not be calculated in compiler
```