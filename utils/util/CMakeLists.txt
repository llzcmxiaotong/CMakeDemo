# 允许endif()的括号内为空，包括foreach, while等等
set(CMAKE_ALLOW_LOOSE_CONSTRUCTS TRUE)

# 输出指定的变量，利用循环输出
# msg_prefix 每条消息的消息前缀
# var_list 数组变量
function(print_loop msg_prefix var_list)
	foreach (var ${var_list})
		message(STATUS ${msg_prefix} ${var})
	endforeach (var)
endfunction(print_loop)


# 输出单条消息
# msg要输出的消息
function(print_msg msg)
	message(STATUS ${msg})
endfunction(print_msg)


# 获取指定文件夹下的所有文件，也可以使用宏定义来实现
# sub_files 函数名
# recurse是否递归
# result 返回的结果
# dir 指定的目录
function(sub_files recurse result dir)
	set(file_list "")
	if (ENABLE_LOG)
		print_msg("dir is ${dir}")
	endif()
	# 判断是否是文件夹
	if (IS_DIRECTORY ${dir})
		set(children "")
		if (recurse)
			#如果是递归
			file(GLOB_RECURSE children RELATIVE ${dir} ${dir}/*)
		else (NOT recurse)
			#如果不是递归
			file(GLOB children RELATIVE ${dir} ${dir}/*)
		endif (recurse)
		if (ENABLE_LOG)
			print_loop("child is: " "${children}")	
		endif ()
		foreach (child ${children})
			if (NOT IS_DIRECTORY ${child})
				list(APPEND file_list ${child})
			endif (NOT IS_DIRECTORY ${child})
		endforeach (child)
	endif(IS_DIRECTORY ${dir})
	if (ENABLE_LOG)
		print_loop("file item: " "${file_list}")
	endif ()
	set(${result} ${file_list} PARENT_SCOPE)
	if (ENABLE_LOG)
		print_loop("result item: " "${result}")
	endif ()
endfunction(sub_files)


# 递归查找指定目录下的子目录,包含自己，这里涉及递归调用，函数的作用域是本函数内有效，递归中失效，而宏是全局有效，所以这里采用了宏定义，通过实验，函数定义没有成功，这个问题困扰我比较长的时间
# sub_dirs_recurse_include_self 宏定义名
# reslult 结果返回值
# root_dir 指定的目录
macro(sub_dirs_recurse_include_self result root_dir)

	if (ENABLE_LOG)
		print_msg("root dir: ${root_dir}")
	endif ()

	if (IS_DIRECTORY ${root_dir})
		# 将根目录加入到result中
		list(APPEND result ${root_dir})

		# 查找根目录下的子目录和文件（非递归,如果这里写成GLOB_RECURSE，得到是文件，没有目录）
		file(GLOB children RELATIVE ${root_dir} ${root_dir}/*)

		if (ENABLE_LOG)
			print_loop("child: " "${children}")
		endif ()
			
		foreach (child ${children})
			if (IS_DIRECTORY ${root_dir}/${child})
				if (ENABLE_LOG)
					print_msg("child dir: ${root_dir}/${child}")
				endif ()
				sub_dirs_recurse_include_self("${result}" ${root_dir}/${child})
				if (ENABLE_LOG)
					print_msg("---------------")
					print_loop("dir item: " "${result}")
				endif ()
			endif ()
		endforeach ()
	endif ()
endmacro ()


# 递归查找指定目录下的子目录
# sub_dirs_recurse 宏定义名
# reslult 结果返回值
# root_dir 指定的目录
macro(sub_dirs_recurse result root_dir)
	if (ENABLE_LOG)
		print_msg("root_dir: ${root_dir}")
	endif ()
	
	if (IS_DIRECTORY ${root_dir})
		file(GLOB children RELATIVE ${root_dir} ${root_dir}/*)
		foreach (child ${children})
			if (IS_DIRECTORY ${root_dir}/${child})
				sub_dirs_recurse_include_self("${result}" ${root_dir}/${child})
			endif ()
		endforeach ()
	endif ()
endmacro ()		
		


# 非递归查找指定目录下的子目录，这里也可以使用function来实现
# sub_dirs 宏定义名
# include_self 是否包含自己
# recurse 是否递归
# reslult 结果返回值
# root_dir 指定的目录
macro(sub_dirs include_self result root_dir)
	if (ENABLE_LOG)
		print_msg("root dir: ${root_dir}")
	endif ()

	if (IS_DIRECTORY ${root_dir})
		if (${include_self})
			list(APPEND result ${root_dir})
		endif ()
		
		file(GLOB children RELATIVE ${root_dir} ${root_dir}/*)
		
		if (ENABLE_LOG)
			print_loop("child is: " "${children}")
		endif ()

		foreach (child ${children})
			if (IS_DIRECTORY ${root_dir}/${child})
				list(APPEND result ${root_dir}/${child})
			endif ()
		endforeach ()
	endif ()
endmacro ()


