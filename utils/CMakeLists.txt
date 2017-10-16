cmake_minimum_required(VERSION 2.8)

configure_file("${PROJECT_SOURCE_DIR}/config.h.in" "${PROJECT_SOURCE_DIR}/config.h")
option(ENABLE_LOG "enable log" ON)

#包含其它的CMakeLists.txt
include(util/CMakeLists.txt)

project(Utils)

message(STATUS "source dir: ${PROJECT_SOURCE_DIR}")

set(result "")
sub_dirs(TRUE result ${PROJECT_SOURCE_DIR})

print_loop("return item: " "${result}")

foreach (f ${result})
	message(STATUS "file is: ${f}")
endforeach (f)

print_msg("cmake command: ${CMAKE_COMMAND}")
