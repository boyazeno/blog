---
layout: default
title: A Small Example
parent: CMakeLists File
grand_parent: C++
nav_order: 1
has_children: false
---

# A Small Example
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## **Example: basic**

```
cmake_minimum_required(Version 3.7.1)

# 项目名称的变量保存到 ${PROJECT_NAME} 变量中
# Keep project name into var ${PROJECT_NAME}
project(HelloWorld)

# 生成一个library 默认为STATIC .o，加入SHARED关键字在名字后面可以构建动态库.so
# 如果不加关键字，可以在使用cmake时加入参数更改默认的库类型： cmake -D BUILD_SHARED_LIBS=TURE .
# Generate a lib, default to STATIC .o. If add keyword SHARED after lib name, could build dynamic link lib .so. 
# If don't add SHARED keyword here, you could still change default lib type to dynamic by runing: cmake -D BUILD_SHARED_LIBS=True  
add_library(my-library src1.cpp src2.cpp)

# 构建可执行文件，使用名字: ${PROJECT_NAME} (即 HelloWorld)
# 使用的源码也要列举
# Build executable, use ${PROJECT_NAME} as name
# List all source code 
add_executable(${PROJECT_NAME} main.cpp)

# 连接需要的library, 
# Link necessary lib.
target_link_libraries(${PROJECT_NAME} PRIVATE my-library)

# 设置install默认路径的prefix 为${PROJECT_SOURCE_DIR}/install
# 路径的全称为：当前的子工程文件夹的根目录/install
# Set default install prefix to ${PROJECT_SOURCE_DIR}/install
set(${CMAKE_INSTALL_PREFIX} ${PROJECT_SOURCE_DIR}/install)

# 将构建好的可执行程序安装到目标位置（DESTINATION）bin中
# 路径全称为：${CMAKE_INSTALL_PREFIX}/bin
# Install executable file to ${CMAKE_INSTALL_PREFIX}/bin
install(TARGETS ${PROJECT_NAME} DESTINATION bin)

# 将源码安装到src的位置
# 路径全称为：${CMAKE_INSTALL_PREFIX}/src
# Install source code to ${CMAKE_INSTALL_PREFIX}/src
install(FILES main.cpp DESTINATION src)

```
[Another example with parameter setting (带有参数设定的额外例子)](https://blog.csdn.net/u010122972/article/details/78216013)
