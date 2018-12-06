cmake_minimum_required(VERSION 3.1)

add_executable(MakeDesc
	"${CMAKE_CURRENT_SOURCE_DIR}/src/MakeDesc.cpp"
	"${CMAKE_CURRENT_SOURCE_DIR}/src/MakeDescAux.cpp")
target_include_directories(MakeDesc PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}/../../include")
