# Table of Contents

* [Import Manually](#import-manually)
* [Use CMake to build a library](#use-cmake-to-build-a-library)
* [Use CMake as a dependency](#use-cmake-as-a-dependency)
* [Use CMake to generate project](#use-cmake-to-generate-project)
* [Compile Flags](#compile-flags)


------
# Import Manually
`yyjson` aims to provide a cross-platform json library, so it was written in ANSI C (actually C99, but compatible with strict C89). You cay copy `yyjson.h` and `yyjson.c` to your project and start using it without any configuration.

If you get compile error, please [report a bug](https://github.com/ibireme/yyjson/issues/new?assignees=&labels=&template=bug_report.md).

# Use CMake to build a library
Clone repository and create build directory:
```shell
git clone https://github.com/ibireme/yyjson.git
mkdir build
cd build
```
Build static library:
```shell
cmake .. 
cmake --build .
```

Build static library and run tests:
```shell
cmake .. -DYYJSON_BUILD_TESTS=ON
cmake --build .
ctest
```

Build shared library:
```shell
cmake .. -DBUILD_SHARED_LIBS=ON
cmake --build .
```

Supported CMake options:

- `-DYYJSON_BUILD_TESTS=ON` Build all tests.
- `-DYYJSON_ENABLE_COVERAGE=ON` Enable code coverage for tests.
- `-DYYJSON_ENABLE_VALGRIND=ON` Enable valgrind memory checker for tests.
- `-DYYJSON_ENABLE_SANITIZE=ON` Enable sanitizer for tests.
- `-DYYJSON_DISABLE_READER=ON` Disable JSON reader if you don't need it.
- `-DYYJSON_DISABLE_WRITER=ON` Disable JSON writer if you don't need it.
- `-DYYJSON_DISABLE_FP_READER=ON` Disable custom double number reader to reduce binary size.
- `-DYYJSON_DISABLE_FP_WRITER=ON` Disable custom double number writer to reduce binary size.
- `-DYYJSON_DISABLE_COMMENT_READER=ON` Disable non-standard comment support at compile time.
- `-DYYJSON_DISABLE_INF_AND_NAN_READER=ON` Disable non-standard nan/inf support at compile time.

See [compile flags](#compile-flags) for details.

# Use CMake as a dependency

You may add the `yyjson` subdirectory to your CMakeFile.txt, and link it to your target:
```cmake
add_subdirectory(vendor/yyjson)
target_link_libraries(your_target yyjson)
```

You may also add some options for yyjson library:
```cmake
set(YYJSON_DISABLE_COMMENT_READER ON CACHE INTERNAL "")
set(YYJSON_DISABLE_INF_AND_NAN_READER ON CACHE INTERNAL "")
add_subdirectory(vendor/yyjson)
target_link_libraries(your_target yyjson)
```

# Use CMake to generate project
If you want to build or debug `yyjson` with other compiler or IDE, try these commands:
```shell
# Clang for Linux/Unix:
cmake .. -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++

# Intel ICC for Linux/Unix:
cmake .. -DCMAKE_C_COMPILER=icc -DCMAKE_CXX_COMPILER=icpc

# Other version of GCC:
cmake .. -DCMAKE_C_COMPILER=/usr/local/gcc-8.2/bin/gcc -DCMAKE_CXX_COMPILER=/usr/local/gcc-8.2/bin/g++

# Microsoft Visual Studio for Windows:
cmake .. -G "Visual Studio 16 2019" -A x64
cmake .. -G "Visual Studio 16 2019" -A Win32
cmake .. -G "Visual Studio 15 2017 Win64"

# Xcode for macOS:
cmake .. -G Xcode

# Xcode for iOS:
cmake .. -G Xcode -DCMAKE_SYSTEM_NAME=iOS

# Xcode with XCTest
cmake .. -G Xcode -DYYJSON_BUILD_TESTS=ON
```


# Compile Flags
`yyjson` support some compile flags:

●**YYJSON_DISABLE_READER**<br/>
Define it as 1 to disable the JSON reader.<br/>
This may reduce binary size if you don't need read JSON.<br/>
These functions will be disabled by this flag:

```
yyjson_read_opts()
yyjson_read_file()
yyjson_read()
```

●**YYJSON_DISABLE_WRITER**<br/>
Define it as 1 to disable the JSON writer.<br/>
This may reduce binary size if you don't need write JSON.<br/>
These functions will be disabled by this flag:

```
yyjson_write_opts()
yyjson_write_file()
yyjson_write()
yyjson_mut_write_opts()
yyjson_mut_write_file()
yyjson_mut_write()
```

●**YYJSON_DISABLE_FP_READER**<br/>
Define it as 1 to disable custom floating-point number reader.<br/>
`yyjson` implements a high-performance floating-point number reader,<br/>
but the fp reader may cost lots of binary size.<br/>

This flag may disable the custom fp reader, and use libc's `strtod()` instead,<br/>
so this flag may reduce binary size, but slow down floating-point reading speed.<br/>

●**YYJSON_DISABLE_FP_WRITER**<br/>
Define it as 1 to disable custom floating-point number writer.<br/>
`yyjson` implements a high-performance floating-point number writer,<br/>
but the fp writer may cost lots of binary size.<br/>

This flag may disable the custom fp writer, and use libc's `sprintf()` instead,<br/>
so this flag may reduce binary size, but slow down floating-point writing speed.<br/>

●**YYJSON_DISABLE_COMMENT_READER**<br/>
Define it as 1 to disable non-standard comment support in JSON reader at compile time.<br/>
This flag may reduce binary size, and increase reading speed slightly.<br/>
This flag may also invalidate the `YYJSON_READ_ALLOW_TRAILING_COMMAS` option.

●**YYJSON_DISABLE_INF_AND_NAN_READER**<br/>
Define it as 1 to disable non-standard inf and nan literal support in JSON reader at compile time.<br/>
This flag may reduce binary size, and increase reading speed slightly.<br/>
This flag may also invalidate the `YYJSON_READ_ALLOW_INF_AND_NAN` option.

●**YYJSON_EXPORTS**<br/>
Define it as 1 to export symbols when build library as Windows DLL.

●**YYJSON_IMPORTS**<br/>
Define it as 1 to import symbols when use library as Windows DLL.
