# CMake最低版本号要求
cmake_minimum_required(VERSION 2.8)

#项目信息
project(Demo2)

message("project source dir: ${PROJECT_SOURCE_DIR}")
message("project binary dir: ${PROJECT_BINARY_DIR}")

set(Demo_VERSION_MAJOR 1)
set(Demo_VERSION_MINOR 0)

# 加入一个配置头文件，用于处理CMake对源码的设置
configure_file("${PROJECT_SOURCE_DIR}/config.h.in"
 "${PROJECT_SOURCE_DIR}/config.h")

# 是否使用自己的math库
option(USE_MYMATH "Use provided math implementation" ON)

# 如果使用自己的库，需要加入math，如果使用系统库，需要链接-lm
if (USE_MYMATH)
	include_directories("${PROJECT_SOURCE_DIR}/math")
	add_subdirectory(math)
	set(EXTRA_LIBS ${EXTRA_LIBS} math)
else ()
	set(EXTRA_LIBS ${EXTRA_LIBS} m)
endif (USE_MYMATH)

#源码的目录，也可以通过set(SRC_DIR .)
aux_source_directory(. SRC_DIR)

#指定生成目标，这里的Demo是最后生成的可执行文件的名字
add_executable(Demo ${SRC_DIR})

# 添加链接库
target_link_libraries(Demo ${EXTRA_LIBS})
