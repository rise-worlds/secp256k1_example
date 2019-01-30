cmake_minimum_required( VERSION 3.5 )
project(ecc_example)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_EXTENSIONS ON)
set(CXX_STANDARD_REQUIRED ON)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/CMakeModules")

include(GNUInstallDirs)

SET( DEFAULT_HEADER_INSTALL_DIR usr/include/${target} )
SET( DEFAULT_LIBRARY_INSTALL_DIR usr/lib )
SET( DEFAULT_EXECUTABLE_INSTALL_DIR usr/bin )

set(whole_archive_flag "--whole-archive")
set(no_whole_archive_flag "--no-whole-archive")
set(build_id_flag "--build-id")

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14 -pthread -fno-builtin-memcmp")
set(CMAKE_CXX_FLAGS "${CMAKE_C_FLAGS} -Wall -Wunused-result")
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -static-libstdc++ -static-libgcc")

add_subdirectory(secp256k1)

include_directories(secp256k1/upstream/include)
include_directories(secp256k1/upstream/src)
#find_library(libsecp256k1 secp256k1)
#link_directories(${CMAKE_CURRENT_BINARY_DIR}/secp256k1/libsecp256k1.a)

set(SOURCE_FILES ${SOURCE_FILES} main.cpp)

add_executable(ecc_example ${SOURCE_FILES})
add_dependencies(ecc_example secp256k1)

target_link_libraries(ecc_example
        PRIVATE secp256k1 ${CMAKE_DL_LIBS})