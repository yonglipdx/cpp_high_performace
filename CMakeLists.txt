cmake_minimum_required(VERSION 3.2)

set(DEBUG_MODE ON)

# Use hunter to download and build some third party dependencies

# update HunterGate to the latest version when needed: see: 
# https://hunter.readthedocs.io/en/latest/quick-start/boost-components.html#first-step

# mkdir cmake
# wget https://raw.githubusercontent.com/cpp-pm/gate/master/cmake/HunterGate.cmake -O cmake/HunterGate.cmake

include("cmake/HunterGate.cmake")

#update hunter archives version/hash by looking at hunter release: https://github.com/cpp-pm/hunter/releases

huntergate(URL "https://github.com/cpp-pm/hunter/archive/v0.25.5.tar.gz" SHA1 "a20151e4c0740ee7d0f9994476856d813cdead29")
#huntergate(URL "https://github.com/cpp-pm/hunter/archive/v0.23.279.tar.gz" SHA1 "0a8ede485c3e9c1ceed8ccb989ab6c0aba1f4db7")
#untergate(URL "https://github.com/cpp-pm/hunter/archive/v0.23.297.tar.gz" SHA1 "3319fe6a3b08090df7df98dee75134d68e2ef5a3")
#HunterGate( URL "https://github.com/path/to/hunter/archive.tar.gz" SHA1 "798501e983f14b28b10cda16afa4de69eee1da1d" )
#huntergate(URL "https://github.com/cpp-pm/hunter/archive/v0.23.224.tar.gz" SHA1 "18e57a43efc435f2e1dae1291e82e42afbf940be")

project(CppHighPerformanceCodeExamples)

set_property(GLOBAL PROPERTY USE_FOLDERS ON)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED YES)
set(CMAKE_CXX_EXTENSIONS NO)

if("${CMAKE_CXX_COMPILER_ID}" STREQUAL "MSVC")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W4 /await")
endif()

if(DEBUG_MODE)
  message("*************************MSVC Done\n\n")
else()
  message("Skip debug")
endif()


if("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Wno-unused-variable -Wno-unused-but-set-variable")
  if("${CMAKE_CXX_COMPILER_VERSION}" VERSION_GREATER_EQUAL 10.2)
    # Don't bother adding the coroutine flag when using GCC 10.0 or 10.1
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fcoroutines")
  endif()
endif()

if("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Wno-unused-parameter -Wno-unused-variable -Wno-unused-private-field")
  if(${CMAKE_CXX_COMPILER_VERSION} VERSION_GREATER_EQUAL 10)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fcoroutines-ts")
  endif()
endif()


hunter_add_package(Boost)
message("************************* Boost Done\n\n")


hunter_add_package(GTest)
message("************************* GTest Done\n\n")

hunter_add_package(benchmark)
message("************************* benchmark Done\n\n")

find_package(Boost CONFIG REQUIRED)
find_package(GTest CONFIG REQUIRED)
find_package(benchmark CONFIG REQUIRED)

add_subdirectory("Chapter01")
add_subdirectory("Chapter02")
add_subdirectory("Chapter02/benchmarks")
add_subdirectory("Chapter03")
add_subdirectory("Chapter03/benchmarks")
add_subdirectory("Chapter04")
add_subdirectory("Chapter05")
add_subdirectory("Chapter05/benchmarks")
add_subdirectory("Chapter06")
add_subdirectory("Chapter07")
add_subdirectory("Chapter08")
add_subdirectory("Chapter09")
add_subdirectory("Chapter10")
add_subdirectory("Chapter10/benchmarks")
add_subdirectory("Chapter11")
add_subdirectory("Chapter12")
add_subdirectory("Chapter13")
add_subdirectory("Chapter14")
add_subdirectory("Chapter14/benchmarks")

enable_testing()
add_test(NAME Chapter01-A_Brief_Introduction_to_C++ COMMAND Chapter01-A_Brief_Introduction_to_C++)
add_test(NAME Chapter02-Essential_C++_Techniques COMMAND Chapter02-Essential_C++_Techniques)
add_test(NAME Chapter03-Measuring_Performance COMMAND Chapter03-Measuring_Performance)
add_test(NAME Chapter04-Data_Structures COMMAND Chapter04-Data_Structures)
add_test(NAME Chapter05-Algorithms COMMAND Chapter05-Algorithms)
add_test(NAME Chapter06-Ranges_And_Views COMMAND Chapter06-Ranges_And_Views)
add_test(NAME Chapter07-Memory_Management COMMAND Chapter07-Memory_Management)
add_test(NAME Chapter08-Compile_Time_Programming COMMAND Chapter08-Compile_Time_Programming)
add_test(NAME Chapter09-Essential_Utilities COMMAND Chapter09-Essential_Utilities)
add_test(NAME Chapter10-Proxy_Objects_And_Lazy_Evaluation COMMAND Chapter10-Proxy_Objects_And_Lazy_Evaluation)
add_test(NAME Chapter11-Concurrency COMMAND Chapter11-Concurrency)
add_test(NAME Chapter12-Coroutines_And_Lazy_Generators COMMAND Chapter12-Coroutines_And_Lazy_Generators)
add_test(NAME Chapter13-Asynchronous_Programming_With_Coroutines COMMAND Chapter13-Asynchronous_Programming_With_Coroutines)
add_test(NAME Chapter14-Parallel_Algorithms COMMAND Chapter14-Parallel_Algorithms)
