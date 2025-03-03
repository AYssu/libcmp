cmake_minimum_required(VERSION 3.21)

project(scan)

set(CMAKE_BUILD_TYPE Release)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF) 

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC -O3 -fvisibility=hidden")

add_subdirectory(utils)

add_subdirectory(memtool)

add_subdirectory(chainer)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ")

add_executable(${CMAKE_PROJECT_NAME}
  ${CMAKE_CURRENT_SOURCE_DIR}/main.cpp
)

set(PREBUILT_STATIC_LIB_DIR "${CMAKE_CURRENT_SOURCE_DIR}/lib")
add_library(libSea2 STATIC IMPORTED)
set_target_properties(libSea2 PROPERTIES IMPORTED_LOCATION "${CMAKE_CURRENT_SOURCE_DIR}/lib/libSea2.a")
# 内置静态库
add_library(libcmp STATIC IMPORTED)
set_target_properties(libcmp PROPERTIES IMPORTED_LOCATION "${CMAKE_CURRENT_SOURCE_DIR}/lib/libcmp.a")
# 对比静态库 依赖libSea2

target_include_directories(${PROJECT_NAME} PUBLIC
        ${CMAKE_CURRENT_SOURCE_DIR}/include

)
target_link_libraries(${CMAKE_PROJECT_NAME} PRIVATE
  libSea2
  libcmp
  utils
  memtool
  chainer
  z
)
