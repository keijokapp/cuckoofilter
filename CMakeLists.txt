cmake_minimum_required(VERSION 3.5)
project(kolt-networker)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_FLAGS  "${CMAKE_CXX_FLAGS} -fno-strict-aliasing -Wall")

# Dependencies

find_package(OpenSSL REQUIRED)
find_package(Threads REQUIRED)

# Library

add_library(cuckoofilter STATIC "src/hashutil.cc")
target_include_directories(cuckoofilter PUBLIC "src")
file(GLOB PUBLIC_HEADERS "src/*.h")
set_target_properties(cuckoofilter PROPERTIES PUBLIC_HEADER "${PUBLIC_HEADERS}")

# Install

install(TARGETS cuckoofilter EXPORT cuckoofilter-config
	PUBLIC_HEADER DESTINATION include/cuckoofilter
	ARCHIVE DESTINATION lib)
export(TARGETS cuckoofilter FILE cuckoofilter-config.cmake)
install(EXPORT cuckoofilter-config DESTINATION cmake)

# Testing

enable_testing()

add_executable(cuckoofilter-test "example/test.cc")
target_link_libraries(cuckoofilter-test cuckoofilter)
add_test(NAME cuckoofilter-test COMMAND cuckoofilter-test)
