cmake_minimum_required(VERSION 3.5)

project(hello_library_demo)

############################################################
# Create a library
############################################################

#Generate the shared library from the library sources
add_library(hello_library SHARED
    src/Hello.cpp
)
add_library(hello::library ALIAS hello_library)
target_include_directories(hello_library
    PUBLIC
    ${PROJECT_SOURCE_DIR}/include
)

set(SOURCES
    src/main.cpp
)
add_executable(hello_binary ${SOURCES})

target_link_libraries(hello_binary 
    PRIVATE
        hello::library
)