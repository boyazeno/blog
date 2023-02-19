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

constexpr int x5 = x3.size(); // good: x3 is calculated during compilation, thus size is a constexpr.

std::vector<int> x4;
x4.push_back(1);
constexpr int x6 = x4.size(); // compile error: x4 could not be calculated in compiler
```

## Different between `auto&&` and `const auto&`
```cpp
auto&& a = funcReturnAnyValue();
const auto& b = funcReturnAnyValue();
```
Both of them will bind to any type of value.

But `auto&&` will keep the type no matter what it is, e.g. rvalue, lvalue, reference, const or non-const.

For `const auto&`, it will add const to any type it accept.


## Get type of a variable
`decltype` could be used to get the real type of given variable.
```cpp
const string& a{"a"};
decltype(a) b{"b"};
std::cout<< std::is_same<decltype(a), const string&>::value; // 1
std::cout<< std::is_same<decltype(a), string>::value;        // 0
```
But if you want the base class, you could use `std::decay`
```cpp
const string& a{"a"};
decltype(a) b{"b"};
std::cout<< std::is_same<std::decay<decltype(a)>::type, string>::value; // 1
```

Also, for base type comparsion, `typeid` could be used.

But for full type comparsion, please use `std::is_same<>::value`.

```cpp
const string& a{"a"};
string b{"b"};

std::cout<< (typeid(a) == typeid(string));        // 1
```


## Use case for weak_ptr
In c++17 weak_ptr are imported. The core feature for `weak_ptr` is able to observe an object  without take over control.

One use case is when `shared_ptr` are iterativly included in 2 class. Both of them could not be deleted due to having control of each other at the same time.

```cpp
#include <memory>
struct People;
struct City
{
  std::string name;
  std::shared_ptr<People> mayor; // Change to std::weak_ptr<People> mayor;
  City(const std::string& name):name(name){}
  ~City(){printf("Delete city %s\n", name.data());}
  void setMayor(std::shared_ptr<People>& people){mayor = people;}
};

struct People
{
  std::string name;
  std::shared_ptr<City> home;
  People(const std::string& name):name(name){}
  ~People(){printf("Delete people %s\n", name.data());}
  void setHome(std::shared_ptr<City>& city){home = city;}
};

int main()
{
  auto city = std::make_shared<City>("my_city");
  auto mayor = std::make_shared<People>("my_man");
  city->setMayor(mayor);
  mayor->setHome(city);

  return 0;
}
// Suppose to print:
// Delete people my_man
// Delete city my_city
// But due to the nesting structure, neither city nor mayor could be deleted first after program quit.

// Solution, change shared_ptr to weak_ptr.
// Because weak_ptr won't increase mayor->use_count(). So mayor could be deleted directly after use.
```

In order to get control of the weak_ptr, you have to call explicitly:
```cpp
std::shared_ptr<MyClass> my_class_sptr;
std::weak_ptr<MyClass> my_class_wptr{my_class_sptr};
std::shared_ptr<MyClass>  my_class_wptr_control = my_class_wptr.lock();
```

## Use structured bindings to get value from tuple, pair, map, array:
After c++17, structured bindings could be used to easily get control of elements in both `copy, const, &, const&` way.

Even public class member could be bind in this way.
```cpp
#include <iostream>
#include <vector>
#include <tuple>
#include <map>

int main()
{
  // case 1
  {
    std::tuple<int,std::string> my_tuple{1,"a"};
    const auto [a,b] = my_tuple;
    std::cout<<a<<b<<std::endl; //print: 1a
  }
  // case 2
  {
    std::map<int, std::string> my_map{ {1,"a"},{2,"b"} };

    // Read single
    auto [a,b] = *(++my_map.begin());
    std::cout<<a<<b<<std::endl; //print: 2b
    
    // Read iteratively
    for(const auto & [i,j] : my_map)
    {
        std::cout<<i<<j<<std::endl; 
    }
    // print: 1a
    // print: 2b
  }
  
  // case 3
  {
      std::string my_array[2]{"a","b"};
      auto [a,b] = my_array;
      std::cout<<a<<b; //print: ab
  }

  return 0;
}
```