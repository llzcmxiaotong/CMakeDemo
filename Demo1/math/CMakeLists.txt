# 查找当前目录下的所有源文件
# 并将它保存在SUB_DIR_SRC中
aux_source_directory(. SUB_DIR_SRC)

# 生成链接库
add_library(math ${SUB_DIR_SRC})

