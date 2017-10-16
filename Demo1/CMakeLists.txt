# CMake最低版本号要求
cmake_minimum_required(VERSION 2.8)

#项目信息
project(Demo1)

#源码的目录
aux_source_directory(. SRC_DIR)

#添加 math 子目录
add_subdirectory(math)

#指定生成目标，这里的Demo是最后生成的可执行文件的名字
add_executable(Demo ${SRC_DIR})

# 添加链接库
target_link_libraries(Demo math)
