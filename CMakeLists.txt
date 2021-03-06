cmake_minimum_required(VERSION 3.0)
project(relocforests)

option(BUILD_EXAMPLE "Build example application" ON)

if(MSVC)
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W4")
else()
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Wpedantic -std=c++11")
endif()

find_package(OpenCV REQUIRED)

include_directories(include/)
include_directories("$ENV{EIGEN_DIR}") # set EIGEN_DIR env variable

file(GLOB PROJECT_HEADERS	include/*.hpp)
file(GLOB PROJECT_SOURCES	source/MeanShift.cpp)

source_group("Headers" FILES ${PROJECT_HEADERS})
source_group("Sources" FILES ${PROJECT_SOURCES})

add_library(${PROJECT_NAME} ${PROJECT_SOURCES} ${PROJECT_HEADERS})
target_link_libraries(${PROJECT_NAME} ${OpenCV_LIBS})


if(BUILD_EXAMPLE)
	add_executable(${PROJECT_NAME}_example source/main.cpp)
	target_link_libraries(${PROJECT_NAME}_example ${PROJECT_NAME} ${OpenCV_LIBS})
endif(BUILD_EXAMPLE)
