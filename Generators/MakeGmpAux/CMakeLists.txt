cmake_minimum_required(VERSION 3.1)

find_package(GMP REQUIRED)

add_executable(MakeGmpAux
	"${CMAKE_CURRENT_SOURCE_DIR}/src/MakeGmpAux.cpp")
add_dependencies(MakeGmpAux desc_header)
target_include_directories(MakeGmpAux PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}/../../include")
target_include_directories(MakeGmpAux PRIVATE "${CMAKE_CURRENT_BINARY_DIR}/../..")
target_include_directories(MakeGmpAux PRIVATE ${GMP_INCLUDES})
target_link_libraries(MakeGmpAux ${GMP_LIBRARIES})

