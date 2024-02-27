cmake_minimum_required(VERSION 3.5)

# Set the project name
project (third_party_include_demo)

# find a boost install with the libraries filesystem and system
find_package(Boost 1.4.6.1 REQUIRED COMPONENTS filesystem system)



if(Boost_FOUND)
    message ("ubuntu boost found")
else()
    message (FATAL_ERROR "Cannot find boost")
endif()
add_executable(third_party_include_demo main.cpp)

target_link_libraries(third_party_include_demo
    PRIVATE
        Boost::filesystem
)
