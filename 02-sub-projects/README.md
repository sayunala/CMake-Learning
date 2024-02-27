# CMake Sub-Projects

本例展示了如何设置一个包含子项目的 CMake 项目。顶层 CMakeLists.txt 会调用子目录中的 CMakeLists.txt，以创建以下内容：

+ sublibrary1 - 静态库
+ sublibrary2 - 一个只有头文件的库
+ subbinary - 一个可执行程序

## Sublibrary1 - 静态库

