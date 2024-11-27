cmake_minimum_required(VERSION 3.11)

# Set C++ standard to C++14
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Include necessary directories
file(GLOB_RECURSE gc_srcs "${CMAKE_CURRENT_SOURCE_DIR}/src/*.cc")
file(GLOB_RECURSE gc_headers "${CMAKE_CURRENT_SOURCE_DIR}/include/*.h")

set (GC_INCLUDE_DIRS "")
foreach (_header_file ${gc_headers})
    get_filename_component(_dir ${_header_file} DIRECTORY)
    list(APPEND GC_INCLUDE_DIRS ${_dir})
endforeach()
list(REMOVE_DUPLICATES GC_INCLUDE_DIRS)

# CUDA source files
file(GLOB_RECURSE gc_cuda_srcs "${CMAKE_CURRENT_SOURCE_DIR}/src/*.cu")

# Set CUDA architecture
set(CUDA_NVCC_FLAGS "${CUDA_NVCC_FLAGS} -arch=sm_60")

# Include the necessary directories
include_directories(${GC_INCLUDE_DIRS})

# Add the libraries
add_library(lib STATIC ${gc_srcs})
add_library(cuda_lib STATIC ${gc_cuda_srcs})
target_include_directories(cuda_lib PRIVATE ${GC_INCLUDE_DIRS})

# Link with CUDA
find_package(CUDA REQUIRED)
