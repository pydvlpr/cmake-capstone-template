# CMake and Capstone disassembler

This is a sample template for using `CMake` to build source code using the [Capstone disassembler](http://www.capstone-engine.org/).  
It is supposed that `Capstone` is not installed on the build system, but should be downloaded and locally build and linked using `CMake`. 

This example is based on the official example code [C tutorial for Capstone](http://www.capstone-engine.org/lang_c.html) and just contains the basic project structure with a sample `CMakeLists.txt` file.  
You should be able to easily adapt this sample with your own projects code. 

## Project structure

The sample project directory structure looks like

```
.
├── build
└── source
    ├── CMakeLists.txt
    └── test1.c
```

## Create the CMakeLists.txt

The CMakeList has to declare a dependency to `Capstone`.

```
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

```

Then the executable has to be defined.
```
add_executable(test1 test1.c)
```
The `Capstone` library has to be linked against the executable.

```
target_link_libraries(
  test1
  capstone
)
```

## Prepare the build files

A first the build files have to be prepared.

```
-> % cmake -S source -B build
Enabling CAPSTONE_ARM_SUPPORT
Enabling CAPSTONE_ARM64_SUPPORT
Enabling CAPSTONE_M68K_SUPPORT
Enabling CAPSTONE_MIPS_SUPPORT
Enabling CAPSTONE_PPC_SUPPORT
Enabling CAPSTONE_SPARC_SUPPORT
Enabling CAPSTONE_SYSZ_SUPPORT
Enabling CAPSTONE_XCORE_SUPPORT
Enabling CAPSTONE_X86_SUPPORT
Enabling CAPSTONE_TMS320C64X_SUPPORT
Enabling CAPSTONE_M680X_SUPPORT
Enabling CAPSTONE_EVM_SUPPORT
Enabling CAPSTONE_MOS65XX_SUPPORT
Enabling CAPSTONE_WASM_SUPPORT
Enabling CAPSTONE_BPF_SUPPORT
Enabling CAPSTONE_RISCV_SUPPORT
Enabling CAPSTONE_SH_SUPPORT
Enabling CAPSTONE_TRICORE_SUPPORT
-- Configuring done (0.2s)
-- Generating done (0.0s)
-- Build files have been written to: /Users/me/codework/cmake/cmake capstone/c sample/build
```

## Build the project

The project with `capstone` can be build.

```
-> % cmake --build .         
[  1%] Building C object _deps/capstone-build/CMakeFiles/capstone.dir/cs.c.o
[  2%] Building C object _deps/capstone-build/CMakeFiles/capstone.dir/Mapping.c.o
[  3%] Building C object _deps/capstone-build/CMakeFiles/capstone.dir/MCInst.c.o
...
[ 96%] Building C object _deps/capstone-build/CMakeFiles/capstone.dir/arch/TriCore/TriCoreModule.c.o
[ 97%] Linking C static library libcapstone.a
[ 97%] Built target capstone
[ 98%] Building C object CMakeFiles/test1.dir/test1.c.o
[100%] Linking C executable test1
[100%] Built target test1
```

# Run the test program

Finally the test program can be run. Using `capstone` works.

```
-> % ./test1
0x1000: push            rbp
0x1001: mov             rax, qword ptr [rip + 0x13b8]
```
