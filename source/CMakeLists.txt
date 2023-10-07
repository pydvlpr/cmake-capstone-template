cmake_minimum_required(VERSION 3.14)
project(capstone-test)

# GoogleTest requires at least C++14
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include(FetchContent)
FetchContent_Declare(
  capstone
  GIT_REPOSITORY https://github.com/capstone-engine/capstone.git
  GIT_TAG 097c04d9413c59a58b00d4d1c8d5dc0ac158ffaa #current version 5.0.1 tag hash
)
FetchContent_MakeAvailable(capstone)


add_executable(test1 test1.c)

target_link_libraries(
  test1
  capstone
)

