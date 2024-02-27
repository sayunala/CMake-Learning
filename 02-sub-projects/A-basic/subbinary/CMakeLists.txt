project(subbinary_demo)
add_executable(${PROJECT_NAME} main.cpp)
target_link_libraries(${PROJECT_NAME}
    PRIVATE
        sub::lib1
        sub::lib2
)