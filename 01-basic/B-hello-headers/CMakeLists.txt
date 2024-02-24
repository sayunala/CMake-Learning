# VERSION 3.5
cmake_minimum_required(VERSION 3.5)

# PROJECT_NAME hello_headers_demo
project(hello_headers_demo)

# file(GLOB SOURCES "src/*.cpp")
set(SOURCES
    src/Hello.cpp
    src/main.cpp
)

add_executable(${PROJECT_NAME} ${SOURCES})

target_include_directories(${PROJECT_NAME}
    PRIVATE
        ${PROJECT_SOURCE_DIR}/include
)