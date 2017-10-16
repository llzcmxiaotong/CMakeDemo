
message(STATUS "add test case for myTest")

add_test(myTest ${PROJECT_BINARY_DIR}/bin/Demo 2 3)
enable_testing()
