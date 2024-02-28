# CMake Sub-Projects

本例展示了如何设置一个包含子项目的 CMake 项目。顶层 CMakeLists.txt 会调用子目录中的 CMakeLists.txt，以创建以下内容：

+ sublibrary1 - 静态库
+ sublibrary2 - 一个只有头文件的库
+ subbinary - 一个可执行程序

```
$ tree
.
├── CMakeLists.txt
├── subbinary
│   ├── CMakeLists.txt
│   └── main.cpp
├── sublibrary1
│   ├── CMakeLists.txt
│   ├── include
│   │   └── sublib1
│   │       └── sublib1.h
│   └── src
│       └── sublib1.cpp
└── sublibrary2
    ├── CMakeLists.txt
    └── include
        └── sublib2
            └── sublib2.h
```

## 添加一个子目录

一个CMakeLists.txt文件可以包含(include)和调用(call)包含CMakeList.txt文件的子目录

```cmake
add_subdirectory(subbinary)
add_subdirectory(sublibrary1)
add_subdirectory(sublibrary2)
```

## 引用子项目目录（Referencing Sub-Project Directories）

​		当一个项目是使用`project()`命令创建时，CMake和自动创建一组变量用于引用该项目信息。这些变量之后可以被其他子项目或者主项目使用。例如为了引用不同项目的源文件目录可以使用如下命令:

```cmake
${sublibrary1_SOURCE_DIR}
${sublibrary2_SOURCE_DIR}
```

被CMake创建的变量包括：

| Variable           | Info                                                         |
| ------------------ | ------------------------------------------------------------ |
| PROJECT_NAME       | 当前`project()`设置的项目名称。                              |
| CMAKE_PROJECT_NAME | the name of the first project set by the `project()` command, i.e. the top level project.由 project() 命令设置的第一个项目的名称，即顶层项目。 |
| PROJECT_SOURCE_DIR | The source director of the current project.当前项目的源目录。 |
| PROJECT_BINARY_DIR | The build directory for the current project.当前项目的构建目录 |
| name_SOURCE_DIR    | The source directory of the project called "name". In this example the source directories created would be `sublibrary1_SOURCE_DIR`, `sublibrary2_SOURCE_DIR`, and `subbinary_SOURCE_DIR` |
| name_BINARY_DIR    | The binary directory of the project called "name". In this example the binary directories created would be `sublibrary1_BINARY_DIR`, `sublibrary2_BINARY_DIR`, and `subbinary_BINARY_DIR` |



## Sublibrary1 - 静态库

如果一个子项目创建了一个库，其他项目可以通过 调用 `target_link_libraries()` 来引用它。这一位置不需要引用新库全路径并且它也会被当作以来添加

```cmake
target_link_library(subbinary
	PUBLIC
		sublibrary1
)
```

同样的，也可以在只读的上下文中创建一个引用该目标的别名目标

```cmake
add_library(sublibrary2)
add_library(sub::lib2 ALIAS sublibrary2)
```

为了引用别名可以使用下面的语句

```cmake
target_link_library(subbinary
	sub::lib2
)
```

## Sublibrary2 - 只有头文件的库

如果你有一个仅作为头文件创建的库，cmake 支持 INTERFACE 目标，允许创建一个没有任何编译输出的目标。

```cmake
add_library(sublibrary2 INTERFACE)
```

创建目标时，您还可以使用 INTERFACE 作用域包含目标的目录。INTERFACE 作用域用于制定目标需求，这些需求会在链接该目标的任何库中使用，但不会在编译目标本身时使用。

```cmake
target_include_directories(sublibrary2 INTERFACE ${PROJECT_SOURCE_DIR}/include)
```









