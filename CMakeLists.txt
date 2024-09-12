cmake_minimum_required(VERSION 3.26)
project(CUDA-UINT128 C CXX CUDA)

set(CMAKE_CXX_STANDARD 11)

find_package(CUDAToolkit REQUIRED)



add_executable(CUDA-UINT128-CPU test128cpu.cpp)
target_compile_definitions(CUDA-UINT128-CPU PRIVATE -D__x86_64__=1)


option(USE_CUDASIEVE "Use CUDA Sieve" ON)
if(USE_CUDASIEVE)
find_package(CUDASieve REQUIRED)
#use dummy.cpp to make visualstudio find include directories
add_executable(CUDA-UINT128 test128.cu dummy.cpp)
target_compile_definitions(CUDA-UINT128 PRIVATE -D__CUDA_ARCH__DEVICE_HOST__=1)
target_link_libraries(CUDA-UINT128 CUDASieve::CUDASieve ${CUDA_LIBRARIES} curand ${CUDASieve_LIBRARIES} )
set_property(TARGET CUDA-UINT128 PROPERTY CUDA_SEPARABLE_COMPILATION ON)
set_property(TARGET CUDA-UINT128 PROPERTY CUDA_STANDARD 11)
endif(USE_CUDASIEVE)
