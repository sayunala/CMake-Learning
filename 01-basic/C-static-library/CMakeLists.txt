cmake_minimum_required(VERSION 3.5)

project(hello_library_demo)

add_library(hello_library_demo STATIC 
    src/Hello.cpp
)

target_include_directories(hello_library_demo
    PUBLIC
        ${PROJECT_SOURCE_DIR}/include
)
set(SOURCES
    src/main.cpp
)

add_executable(hello_binary_demo ${SOURCES})

target_link_libraries(hello_binary_demo PRIVATE hello_library_demo)
