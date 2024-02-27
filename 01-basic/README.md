# Cmake

## 1.0 Basic Cmake

### 1.A CMake Header

```cmake
cmake_minimum_required(VERSION 3.5) #最小 CMake 版本 
project (hello_cmake) #项目名称
add_executable(hello_cmake main.cpp) # 创建可执行文件 可以使用add_executable(${PROJECT_NAME} main.cpp) 
```

### 1.B Cpp Header

创建头文件目录

```
B-hello-headers$ tree
.
├── CMakeLists.txt
├── include
│   └── Hello.h
└── src
    ├── Hello.cpp
    └── main.cpp
```

当有多个包含目录时使用target_include_directory()将这些路径连接到目标

```cmake
target_include_directory(targets 
	PRIVATE ${PROJECT_SOURCE_DIR}/include
    )
```

+ **PRIVATE**：表明只有连接到的目标可以访问
+ **PUBLIC**: 表明连接到的目标和以来该目标的其他目标也可以访问

| Variable                 | Info                                                         |
| ------------------------ | ------------------------------------------------------------ |
| CMAKE_SOURCE_DIR         | 源代码根目录路径                                             |
| CMAKE_CURRENT_SOURCE_DIR | 使用子项目或子目录的当前路径                                 |
| PROJECT_SOURCE_DIR       | 当前CMake项目的源代码路径                                    |
| PROJECT_BINARY_DIR       | The build directory for the current project.                 |
| CMAKE_BINARY_DIR         | The root binary / build directory. This is the directory where you ran the cmake command. |
| CMAKE_CURRENT_BINARY_DIR | 当前在的构建路径                                             |



### 1.C Cpp Static Library

创建并链接到静态库

```
B-hello-headers$ tree
.
├── CMakeLists.txt
├── include
│   └── Hello.h
└── src
    ├── Hello.cpp
    └── main.cpp
```

通过add_library()

```cmake
add_library(static_library STATIC src/Hello.cpp)
```



### 1.E Cpp Shared Library

```cmake
add_library(share_library SHARED src/Hello.cpp)
```

可以`add_library`使用**ALIAS**为`targets`声明一个别名如：

```cmake
add_library(share::library ALIAS)
```

之后可以通过`share::library`表明库如

```cmake
target_link_library(exe PRIVATE share::library)
```

### 1.F Installing

```

$ tree
.
├── cmake-examples.conf
├── CMakeLists.txt
├── include
│   └── installing
│       └── Hello.h
├── README.adoc
└── src
    ├── Hello.cpp
    └── main.cpp
```

CMake 通过CMAKE_INSTALL_PREFIX设置安装路径。可以通过如下命令行控制安装位置

```bash
cmake .. -DCMAKE_INSTALL_PREFIX=/install/location
```

同时也可以使用CMake GUI进行控制

CMake 通过 `install()`函数安装如下

```cmake
add_execable
# 安装二进制目标 binary 到${CMAKE_INSTALL_PREFIX}/bin
install(TARGETS binary DESTINATION bin)

# 安装library 到${CMAKE_INSTALL_PREFIX}/lib
install(TARGETS library LIBRARY DESTINATION lib)

# 安装include 目录到${CMAKE_INSTALL_PREFIX}/include
install(DIRECTORY ${PROJECT_SOURCE_DIR}/include/ DESTINATION include)

# 安装配置文件到 ${CMAKE_INSTALL_PREFIX}/etc
intsall(FILES cmake-examples.conf DESTINATION etc)
```

### 1.G Build Type

CMake 有许多内置的编译配置，可用于编译项目。这些配置指定了优化级别，以及二进制文件中是否包含调试信息。

CMake提供的级别有:

+ Release - 为编译器增加 `-O3 -DNDEBUG`标志
+ Debug - 为编译器增加`-g` 标志
+ MinSizeRel - 为编译器增加 `-Os -DNDEBUG`
+ RelWithDebInfo - 为编译器添加 `-O2 -g -DNDEBUG`标志 

**设置Build Type**

+ 使用CMake GUI设置 **CMake_BUILD_TYPE**

+ 使用命令行

  ```cmake
  cmake .. -DCMAKE_BUILD_TYPE=RELEASE
  ```

设置默认Build Type

```cmake
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  message("Setting build type to 'RelWithDebInfo' as none was specified.")
  set(CMAKE_BUILD_TYPE RelWithDebInfo CACHE STRING "Choose the type of build." FORCE)# 设置默认构建类型
  # Set the possible values of build type for cmake-gui
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release"
    "MinSizeRel" "RelWithDebInfo")
endif()
```

### 1.H Complie Flags

CMake支持以多种不同的方式设置编译标志：

+ 使用`target_compile_definitions()`函数
+ 使用`CMAKE_C_FLAGS ` 和 `CMAKE_CXX_FLAGS`变量

```cmake
$ tree
.
├── CMakeLists.txt
├── main.cpp
```

**为每个目标设置 C++标志**

使用下面的语句可以将导致编译器在编译目标时添加 -DEX3 定义。

如果目标是一个库，并且选择了 PUBLIC 或 INTERFACE 范围，那么该定义也将包含在链接该目标的任何可执行文件中。

```cmake
target_compile_definitions(cmake_examples_compile_flags
    PRIVATE EX3
)
```

对于编译器选项可以使用 `target_compile_options()`函数

**设置默认C++标志**

```cmake
set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DEX2" CACHE STRING "Set C++ Compiler Flags" FORCE)
```

| Note | The values `CACHE STRING "Set C++ Compiler Flags" FORCE` from the above command are used to force this variable to be set in the CMakeCache.txt file.  For more details, see [here](https://cmake.org/cmake/help/v3.0/command/set.html) |      |
| ---- | ------------------------------------------------------------ | ---- |

设置CMake标志

类似于build type可以使用 CMake GUI工具设置

+ 也可以使用:

  ```bash
  cmake .. -DCMKAE_CXX_FLAGS="-DEX3"
  ```

  

  

### 1.I Third Party Library

### 1.J Imported Targets

### 1.K Cpp Standard

