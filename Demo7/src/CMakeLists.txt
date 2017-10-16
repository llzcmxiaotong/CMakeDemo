# CMake最低版本号要求
# cmake_minimum_required(VERSION 2.8)

#项目信息
# project(Demo5)

MACRO(SUBDIRLIST result curdir)
  FILE(GLOB children RELATIVE ${curdir} ${curdir}/*)
  SET(dirlist "")
  FOREACH(child ${children})
    IF(IS_DIRECTORY ${curdir}/${child})
      LIST(APPEND dirlist ${child})
    ENDIF(IS_DIRECTORY ${curdir}/${child})
  ENDFOREACH(child)
  SET(${result} ${dirlist})
ENDMACRO(SUBDIRLIST)

message("project source dir: ${PROJECT_SOURCE_DIR}")
message("project binary dir: ${PROJECT_BINARY_DIR}")

message(STATUS "cmake current dir ${CMAKE_CURRENT_SOURCE_DIR}")
message(STATUS "cmake current path: ${CMAKE_CURRENT_LIST_FILE}")
message(STATUS "cmake module path: ${CMAKE_MODULE_PATH}") 

set(Demo_VERSION_MAJOR 1)
set(Demo_VERSION_MINOR 0)

# 加入一个配置头文件，用于处理CMake对源码的设置
configure_file("${PROJECT_SOURCE_DIR}/config.h.in"
	"${PROJECT_SOURCE_DIR}/src/config.h")

# 是否使用自己的math库
option(USE_MYMATH "Use provided math implementation" ON)

find_path(header math.h)
if (header)
	message(STATUS "find math.h header")
	include_directories(header)
endif (header)

exec_program(ls OUTPUT_VARIABLE output RETURN_VALUE ret_val)

message("output: ${output}") 

# 如果使用自己的库，需要加入math，如果使用系统库，需要链接-lm
if (USE_MYMATH)
	include_directories(/tmp/demo4/include/library)
	# 如果默认写成math，链接的是动态库（当然情况是静态和动态库都存在的情况）
	set(EXTRA_LIBS ${EXTRA_LIBS} libmath.a)
else (!USE_MYMATH)
	set(EXTRA_LIBS ${EXTRA_LIBS} m)
endif (USE_MYMATH)

# 增加编译时的标志，也可以用CMAKE_C_FLAGS和CMAKE_CXX_FLAGS变量
add_definitions(-DENABLE_DEBUG)

#源码的目录，也可以通过set(SRC_DIR .)
# aux_source_directory(. SRC_DIR)
set(SRC_DIR ${CMAKE_CURRENT_SOURCE_DIR})
file(GLOB sourceFile "*.c")

message("sourceFile: ${sourceFile}")

set(currentDir ${CMAKE_CURRENT_SOURCE_DIR})
message("current dir: ${currentDir}")
execute_process(COMMAND ls "${currentDir}/*.c" OUTPUT_VARIABLE LS_OUTPUT)
# if (NOT LS_RVALUE)
	message(STATUS "ls result: ${LS_OUTPUT}")
# endif (NOT LS_RVALUE)

message(STATUS "ls return value: ${LS_RVALUE}")

exec_program(mkdir ARGS -p a RETURN_VALUE LS_RVALUE)
message(STATUS "mkdir return value: ${LS_RVALUE}")

file(WRITE a.txt "message to write\n")
file(APPEND a.txt "append\n")
file(MAKE_DIRECTORY A)
file(TO_CMAKE_PATH abc/abc result)
file(RELATIVE_PATH relativePath ${CMAKE_CURRENT_SOURCE_DIR} ${PROJECT_SOURCE_DIR})
message("relativePath: ${relativePath}")

#添加链接库的路径
link_directories(/tmp/demo4/lib/)

find_library(mylib libmath.a /tmp/demo4/lib/)
if (mylib)
	message(STATUS "find library success")
endif(mylib)

# 它执行的是build目录的目录
if (EXISTS currentDir)
	message(STATUS "exists currentDir file or dir")
endif (EXISTS currentDir)



SUBDIRLIST(subDirList ${PROJECT_SOURCE_DIR})

FILE(GLOB children RELATIVE ${SRC_DIR} ${SRC_DIR}/*)
message(STATUS "SRC_DIR: ${SRC_DIR}")
foreach(f ${children})
	message(STATUS "file is: ${f}")
endforeach(f)

set(srcChildren ${SRC_DIR}/*)
message("src children: ${srcChildren}")

# add_denpenencies的必须是一个已经存在的可执行文件或库
# add_dependencies(Demo ${EXTRA_LIBS})

#指定生成目标，这里的Demo是最后生成的可执行文件的名字
add_executable(Demo ${sourceFile})

# 添加链接库
target_link_libraries(Demo ${EXTRA_LIBS})



