cmake_minimum_required(VERSION 2.8)
  
project(ntl)

# Search path for CMake include files.
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

# Require C++ 11 support
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED TRUE)
set(CMAKE_CXX_EXTENSIONS OFF)

find_package(GMP REQUIRED)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -O2 -pthread -march=native")

file(GLOB SRC
	"src/FFT.cpp"
	"src/FacVec.cpp"
	"src/GF2.cpp"
	"src/GF2E.cpp"
	"src/GF2EX.cpp"
	"src/GF2EXFactoring.cpp"
	"src/GF2X.cpp"
	"src/GF2X1.cpp"
	"src/GF2XFactoring.cpp"
	"src/GF2XVec.cpp"
	"src/GetTime.cpp"
	"src/GetPID.cpp"
	"src/HNF.cpp"
	"src/ctools.cpp"
	"src/LLL.cpp"
	"src/LLL_FP.cpp"
	"src/LLL_QP.cpp"
	"src/LLL_RR.cpp"
	"src/LLL_XD.cpp"
	"src/RR.cpp"
	"src/WordVector.cpp"
	"src/ZZ.cpp"
	"src/ZZVec.cpp"
	"src/ZZX.cpp"
	"src/ZZX1.cpp"
	"src/ZZXCharPoly.cpp"
	"src/ZZXFactoring.cpp"
	"src/ZZ_p.cpp"
	"src/ZZ_pE.cpp"
	"src/ZZ_pEX.cpp"
	"src/ZZ_pEXFactoring.cpp"
	"src/ZZ_pX.cpp"
	"src/ZZ_pX1.cpp"
	"src/ZZ_pXCharPoly.cpp"
	"src/ZZ_pXFactoring.cpp"
	"src/fileio.cpp"
	"src/lip.cpp"
	"src/lzz_p.cpp"
	"src/lzz_pE.cpp"
	"src/lzz_pEX.cpp"
	"src/lzz_pEXFactoring.cpp"
	"src/lzz_pX.cpp"
	"src/lzz_pX1.cpp"
	"src/lzz_pXCharPoly.cpp"
	"src/lzz_pXFactoring.cpp"
	"src/mat_GF2.cpp"
	"src/mat_GF2E.cpp"
	"src/mat_RR.cpp"
	"src/mat_ZZ.cpp"
	"src/mat_ZZ_p.cpp"
	"src/mat_ZZ_pE.cpp"
	"src/mat_lzz_p.cpp"
	"src/mat_lzz_pE.cpp"
	"src/mat_poly_ZZ.cpp"
	"src/mat_poly_ZZ_p.cpp"
	"src/mat_poly_lzz_p.cpp"
	"src/quad_float.cpp"
	"src/quad_float1.cpp"
	"src/tools.cpp"
	"src/vec_GF2.cpp"
	"src/vec_GF2E.cpp"
	"src/vec_RR.cpp"
	"src/vec_ZZ.cpp"
	"src/vec_ZZ_p.cpp"
	"src/vec_ZZ_pE.cpp"
	"src/vec_lzz_p.cpp"
	"src/vec_lzz_pE.cpp"
	"src/xdouble.cpp"
	"src/G_LLL_FP.cpp"
	"src/G_LLL_QP.cpp"
	"src/G_LLL_XD.cpp"
	"src/G_LLL_RR.cpp"
	"src/thread.cpp"
	"src/BasicThreadPool.cpp"
	"src/MatPrime.cpp"
	"src/pd_FFT.cpp")

set(FEATURE_HEADERS
	"${CMAKE_CURRENT_BINARY_DIR}/NTL/HAVE_ALIGNED_ARRAY.h"
	"${CMAKE_CURRENT_BINARY_DIR}/NTL/HAVE_AVX.h"
	"${CMAKE_CURRENT_BINARY_DIR}/NTL/HAVE_AVX2.h"
	"${CMAKE_CURRENT_BINARY_DIR}/NTL/HAVE_AVX512F.h"
	"${CMAKE_CURRENT_BINARY_DIR}/NTL/HAVE_BUILTIN_CLZL.h"
	"${CMAKE_CURRENT_BINARY_DIR}/NTL/HAVE_CHRONO_TIME.h"
	"${CMAKE_CURRENT_BINARY_DIR}/NTL/HAVE_COPY_TRAITS1.h"
	"${CMAKE_CURRENT_BINARY_DIR}/NTL/HAVE_COPY_TRAITS2.h"
	"${CMAKE_CURRENT_BINARY_DIR}/NTL/HAVE_FMA.h"
	"${CMAKE_CURRENT_BINARY_DIR}/NTL/HAVE_LL_TYPE.h"
	"${CMAKE_CURRENT_BINARY_DIR}/NTL/HAVE_MACOS_TIME.h"
	"${CMAKE_CURRENT_BINARY_DIR}/NTL/HAVE_PCLMUL.h"
	"${CMAKE_CURRENT_BINARY_DIR}/NTL/HAVE_POSIX_TIME.h"
	"${CMAKE_CURRENT_BINARY_DIR}/NTL/HAVE_SSSE3.h")

set(DESC_HEADER
	"${CMAKE_CURRENT_BINARY_DIR}/NTL/mach_desc.h")

set(GMP_HEADER
	"${CMAKE_CURRENT_BINARY_DIR}/NTL/gmp_aux.h")

add_custom_command(
	OUTPUT ${FEATURE_HEADERS}
	COMMAND ${CMAKE_COMMAND} -E make_directory "${CMAKE_CURRENT_BINARY_DIR}/NTL"
	COMMAND sh ${CMAKE_CURRENT_SOURCE_DIR}/MakeCheckFeatures "ALIGNED_ARRAY BUILTIN_CLZL LL_TYPE SSSE3 AVX PCLMUL AVX2 FMA AVX512F COPY_TRAITS1 COPY_TRAITS2 CHRONO_TIME MACOS_TIME POSIX_TIME" "${CMAKE_CXX_COMPILER} ${CMAKE_CXX_FLAGS} -I${CMAKE_CURRENT_SOURCE_DIR}/include -I${CMAKE_CURRENT_BINARY_DIR}/" "-lm" "${CMAKE_CURRENT_SOURCE_DIR}/src"
	COMMENT "Analyzing avaialble features ..."
	WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/MakeCheckFeatures ${DESC_HEADER})
set_source_files_properties("${FEATURE_HEADERS}" PROPERTIES GENERATED TRUE)
LIST(APPEND SRC ${FEATURE_HEADERS})

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/Generators/MakeDesc)

add_custom_command(
	OUTPUT ${DESC_HEADER}
	COMMAND ${CMAKE_COMMAND} -E make_directory "${CMAKE_CURRENT_BINARY_DIR}/NTL"
	COMMAND $<TARGET_FILE:MakeDesc>
	COMMENT "Generate machine description ..."
	WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
	DEPENDS MakeDesc)
set_source_files_properties("${DESC_HEADER}" PROPERTIES GENERATED TRUE)
LIST(APPEND SRC ${DESC_HEADER})

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/Generators/MakeGmpAux)

add_custom_command(
	OUTPUT ${GMP_HEADER}
	COMMAND ${CMAKE_COMMAND} -E make_directory "${CMAKE_CURRENT_BINARY_DIR}/NTL"
	COMMAND $<TARGET_FILE:MakeGmpAux> >${GMP_HEADER}
	COMMENT "Generate GMP header ..."
	WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
	DEPENDS MakeGmpAux ${DESC_HEADER})
set_source_files_properties("${GMP_HEADER}" PROPERTIES GENERATED TRUE)
LIST(APPEND SRC ${GMP_HEADER})

add_library(${PROJECT_NAME} SHARED ${SRC})
target_include_directories(${PROJECT_NAME} PRIVATE ${GMP_INCLUDES})
target_include_directories(${PROJECT_NAME} PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/include)
target_include_directories(${PROJECT_NAME} PRIVATE ${CMAKE_CURRENT_BINARY_DIR})
target_compile_definitions(${PROJECT_NAME} PRIVATE NTL_GMP_LIP=on)
target_link_libraries(${PROJECT_NAME} ${GMP_LIBRARIES})

