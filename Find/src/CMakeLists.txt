
add_executable(curltest main.c)

find_package(CURL)
if (CURL_FOUND)
	target_include_directories(curltest PRIVATE ${CURL_INCLUDE_DIR})
	target_link_libraries(curltest ${CURL_LIBRARY})
	# target_link_libraries(curltest curl)
else (CURL_FOUND)
	message(FATAL_ERROR "curl library not found")
endif (CURL_FOUND)

