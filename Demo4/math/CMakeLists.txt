# 查找当前目录下的所有源文件
aux_source_directory(. SUB_DIR_SRC)

set(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)

# 生成链接库
# set_target_properties(math_shared PROPERTIES OUTPUT_NAME "math")
add_library(math_shared SHARED ${SUB_DIR_SRC})

#指定编译语言cc
set_target_properties(math_shared PROPERTIES LINKER_LANGUAGE CXX)
set_target_properties(math_shared PROPERTIES OUTPUT_NAME "math")
set_target_properties(math_shared PROPERTIES VERSION 1.2 SOVERSION 1)

add_library(math_static STATIC ${SUB_DIR_SRC})
set_target_properties(math_static PROPERTIES OUTPUT_NAME "math")

# add_library(math_static STATIC ${SUB_DIR_SRC})
# set_target_properties(math_static PROPERTIES OUTPUT_NAME "math")


install(TARGETS math_shared math_static
	LIBRARY DESTINATION lib
	ARCHIVE DESTINATION lib
	)
install(FILES library.h DESTINATION include/library)
