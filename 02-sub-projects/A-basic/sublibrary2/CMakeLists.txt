cmake_minimum_required(VERSION 3.5)

project(sublibrary2_demo)

add_library(${PROJECT_NAME} INTERFACE)
add_library(sub::lib2 ALIAS ${PROJECT_NAME})
target_include_directories(${PROJECT_NAME}
INTERFACE
        ${PROJECT_SOURCE_DIR}/include
 )