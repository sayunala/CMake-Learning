# Cmake

## 1.0 Basic Cmake

### 1.1 Header

```cmake
cmake_minimum_required(VERSION 3.5) #最小 CMake 版本 
project (hello_cmake) #项目名称
add_executable(hello_cmake main.cpp) # 创建可执行文件 可以使用add_executable(${PROJECT_NAME} main.cpp) 
```

​	

