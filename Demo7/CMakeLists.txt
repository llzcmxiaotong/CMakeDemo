# CMake的最低版本号要求
cmake_minimum_required(VERSION 2.8)

# 工程名字
project(Demo5)

add_subdirectory(src bin)

add_subdirectory(test)

enable_testing()

set(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)
set(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)

message(STATUS "current module path: ${CMAKE_MODULE_PATH}")

message(STATUS "home dir: $ENV{HOME}")
# install(FILES COPYRIGHT README DESTINATION share/doc/cmake/demo3)
# install(PROGRAMS runDemo.sh DESTINATION bin)
# install(DIRECTORY doc/ DESTINATION share/doc/cmake/demo3)
# install(TARGETS Demo library
#	RUNTIME DESTINATION bin
#	ARCHIVE DESTINATION libstatic
#	)
