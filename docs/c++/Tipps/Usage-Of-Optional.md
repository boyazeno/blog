---
layout: default
title: Usage-Of-Optional
parent: Tipps
grand_parent: C++
nav_order: 2
has_children: false
---

# Usage Of Optional
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Introduce
In c++17 optional is introduced in header `#include <optional>`, which allows user to accept optional variables as in/output. 

In general the idea is the same as in/output `None` in python.

## Usage
```cpp
// Create a optional
auto x1 = std::make_optional<int>(1);
auto x2 = std::make_optional<std::vector<int>>({1,2,3});
std::optional<std::vector<int>> x3 = optVec(std::vector<int>(1,1));
std::optional<int> x4 = std::nullopt;

// Check if an optional variable is empty or not
bool flag1 = x1.has_value();
if(x2)
{
    printf("Is not empty\n");
}

// Get value
x2->at(1);
(*x2)[1];
x2.value()[1];

// Throw error
try
{
    x4->value();  // this will throw error
    auto a = *x4; // this will not throw error, it will return the instance of default constructor of T
}
catch(const std::bad_optional_access& e)
{
    std::cout<<e.what(); // print: bad optional access
} 
```

## Tipps:
* It's a bad habit to use input as output.
The following usage is not a good pratices.
```cpp
bool getMultiOutput(MyClass& output_1, MyClass& output_2, const MyClass& input_1, const MyClass& input_2)
{
    // Just do something to create output_1
    output_1 = input_1;
    output_2 = input_2;
    return True;
}
```
Due to the fact that:
1. It's unclear for user, which is output, which is input.
2. Now most of the compiler are capable of optimize the return value to reduce an extra copy, but not for return through input.
3. Error prone. If the output is a vector, but the input vector could be non-empty. This could easily lead to unspecified behaviours.
4. Old.

The right way to do is:
```cpp
#include <iostream>
#include <vector>
#include <tuple>
#include <optional>
using MyType = std::tuple<int, std::optional<std::string>>;

MyType myFunc(const bool& flag)
{
    if(flag)
    {
        return MyType(0,std::nullopt);
    }
    return MyType(1, "flag is not true.");
}

void test(const MyType& res)
{
    if(std::get<1>(res).has_value())
    {
        std::cout<<std::get<1>(res).value()<<std::endl;
    }
    else
    {
        std::cout<<"flag is true"<<std::endl;
    }
}

int main()
{
    const auto& res1 = myFunc(true); // print: flag is true
    const auto& res2 = myFunc(false);// print: flag is not true

    test(res1);
    test(res2);
    
    return 0;
}

```