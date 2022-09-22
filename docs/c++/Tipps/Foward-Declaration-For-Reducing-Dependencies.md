---
layout: default
title: Foward Declaration For Reducing Dependencies
parent: Tipps
grand_parent: C++
nav_order: 3
has_children: false
---

# A Small Example
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

# Uses unique pointer as Impl to reduce compiling dependences

If you are suffering heavily compilition time, it's might be good to replace the some member variables with unique_ptr. (which actually use forward declaration)

The key idea is to reduce using "real object", instead try to use pointer together with incomplete class defination. So the real header file for your member variables won't be necessary when you just use the pointer in the header file.  The real header file only necessary when you really use the object in cpp file.


## Pointer instead of Instance :
For example if you have a class which has a few variables like below:
```cpp
// zoo.h
#include "monkey.h"
#include "duck.h"
#include "sheep.h"
class Zoo
{
  Zoo();

  private:
  Monkey monkey_;
  Duck duck_;
  sheep sheep_;
};
```
In this case, you need to really include those 3 header in order to compiling properly. But actually you don't really need the real defination here, since no real method been called in header file. Everything meaningful happens in cpp file.

But now if something changed in each of these 3 included header files, the zoo.h will also need to be affected.

A good way to do is to replace each real instance to a pointer like this:

```cpp
class Monkey;
class Duck;
class Sheep;

class Zoo
{
  Zoo();
  ~zoo();

  private:
  std::unique_ptr<Monkey> monkey_;
  std::unique_ptr<Duck> duck_;
  std::unique_ptr<sheep> sheep_;
};
```
```cpp
// zoo.cpp
#include "monkey.h"
#include "duck.h"
#include "sheep.h"
#include "zoo.h"

Zoo::Zoo():monkey_(new Monkey), duck_(new Duck), sheep_(new Sheep)
{}
Zoo::~Zoo(){}

```

Now we no longer need to include these 3 header file anymore. Forward declaration helped a lot in this sense.

**Attention**

Be careful when you do the replacement. Here we use unique_ptr, but unique_ptr has it's own default deleter which will be called implicitly when the destructor of Zoo class is been called. If here in header file don't explicitly declare `~Zoo()`, the compiler will assgin us a default one which actually call the default deleter of unique_ptr. Unfortunetly, inside default deleter of unique_ptr, `static_assert` will be used to avoid delete an imcomplete pointer, which cause the compiler throw error (len blabla error). The solution is to make sure the compiler is been told to find the real implementation of the pointed class when it try to do the deletion. 

By declare the destructor in header file force the compiler to find the cpp file first, which also leads to find the real implementation of pointed class.

**Another attention**

If we don't use default destructor, that means no default copy and move constructor is given. That also means you have to implement by yourself. Again unfortunetly, the default move constructor will call pointer deleter implicitly. So, we have to use the same method as above: declare them in header file first, then define them by us in cpp file. (Also, the unique_ptr don't have a copy constructor, so you need toimplement it anyway.)

##  When unique, when shared?

In the example above, we use unique pointer, which requires us to separate the defination and declaration of constructors. But what if we use shared_ptr?

The answer is: the above mentioned separatation is no longer required. The reason is that in shared pointer, the default deleter is not a part of it. When the deleter need to be called, it will first try to find the complete defination of pointed class, use their deleter instead. 

Why not use shared_ptr? The answer is: in some sense, the default deleter makes unique_ptr faster in runtime (smaller). But which one to use more depends on the usecase. 

