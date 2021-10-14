# cmake 最低版本需求
cmake_minimum_required(VERSION 3.13)

# 工程名称
project(cmake_example)

# 设置C++11标准
set(CMAKE_CXX_STANDARD 11)

# 编译源码生成目标：将main.cc的源文件编译成一个名称为cmake_example的可执行文件
add_executable(cmake_example src/main.cc)
