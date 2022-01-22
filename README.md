## CMakeCook笔记

### 第1章：hello world

1、使用对象库生成静态库和动态库：

```bash
➜  chapter01 git:(master) ✗ ls
CMakeFiles       hello_world      libmessage.a     libmessage.dylib
```
2、条件编译

变量的逻辑判断：
- 如果将逻辑变量设置为以下任意一种: 1 、 ON 、 YES 、 true 、 Y 或 非零数，则逻辑变量为 true 。
- 如果将逻辑变量设置为以下任意一种:0、 、NO、false、N、或者以 -NOTFOUND 为后缀，则逻辑变量为 false。


```bash
➜  build git:(master) ✗ cmake -DUSE_LIBRARY=ON ..
-- The CXX compiler identification is AppleClang 11.0.0.11000033
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
Compile source into a library? ON
-- Configuring done
-- Generating done
-- Build files have been written to: /Users/guodongzhu/SourceCode/CMakeCookbook/build
➜  build git:(master) ✗ make
[ 20%] Building CXX object chapter01/CMakeFiles/message-obj.dir/Message.cpp.o
[ 20%] Built target message-obj
[ 40%] Linking CXX static library libmessage.a
[ 40%] Built target message-static
[ 60%] Building CXX object chapter01/CMakeFiles/hello_world.dir/hello_world.cpp.o
[ 80%] Linking CXX executable hello_world
[ 80%] Built target hello_world
[100%] Linking CXX shared library libmessage.dylib
[100%] Built target message-shared
```

`-D` 开关用于为CMake设置任何类型的变量:逻辑变量、路径等等。

option语法：

```
option(<option_variable> "help string" [initial value])
```

选项之间有依赖，使用 `CMakeDependentOption` 模块。

```bash
➜  build git:(master) ✗ cmake --help-module CMakeDependentOption
CMakeDependentOption
--------------------

Macro to provide an option dependent on other options.

This macro presents an option to the user only if a set of other
conditions are true.

Usage:

 cmake_dependent_option(<option> "<help_text>" <value> <depends> <force>)

Where ``<option>`` is available to the user if ``<depends>`` is true. When
``<option>`` is available, the given ``<help_text>`` and initial ``<value>``
are used. If the ``<depends>`` condition is not true, ``<option>`` will not be
presented and will always have the value given by ``<force>``. Any value set by
the user is preserved for when the option is presented again. Each element in
the fourth parameter is evaluated as an if-condition, so
:ref:`Condition Syntax` can be used.

Example invocation:

 cmake_dependent_option(USE_FOO "Use Foo" ON
                        "USE_BAR;NOT USE_ZOT" OFF)

If ``USE_BAR`` is true and ``USE_ZOT`` is false, this provides an option called
``USE_FOO`` that defaults to ON. Otherwise, it sets ``USE_FOO`` to OFF and
hides the option from the user. If the status of ``USE_BAR`` or ``USE_ZOT``
ever changes, any value for the ``USE_FOO`` option is saved so that when the
option is re-enabled it retains its old value.
```

指定编译器

CMake将语言的编译器存储在 `CMAKE_<LANG>_COMPILER` 变量中，其
中 `<LANG>` 是受支持的任何一种语言。

```bash
➜  build git:(master) ✗ cmake -DCMAKE_CXX_COMPILER=clang++ ..

➜  build git:(master) ✗ make VERBOSE=1
/usr/local/Cellar/cmake/3.21.2/bin/cmake -S/Users/guodongzhu/SourceCode/CMakeCookbook -B/Users/guodongzhu/SourceCode/CMakeCookbook/build --check-build-system CMakeFiles/Makefile.cmake 0
...省略...
[ 33%] Building CXX object chapter01/CMakeFiles/hello_world.dir/hello_world.cpp.o
cd /Users/guodongzhu/SourceCode/CMakeCookbook/build/chapter01 && /usr/bin/clang++   -isysroot /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.15.sdk -MD -MT chapter01/CMakeFiles/hello_world.dir/hello_world.cpp.o -MF CMakeFiles/hello_world.dir/hello_world.cpp.o.d -o CMakeFiles/hello_world.dir/hello_world.cpp.o -c /Users/guodongzhu/SourceCode/CMakeCookbook/chapter01/hello_world.cpp
[ 66%] Building CXX object chapter01/CMakeFiles/hello_world.dir/Message.cpp.o
cd /Users/guodongzhu/SourceCode/CMakeCookbook/build/chapter01 && /usr/bin/clang++   -isysroot /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.15.sdk -MD -MT chapter01/CMakeFiles/hello_world.dir/Message.cpp.o -MF CMakeFiles/hello_world.dir/Message.cpp.o.d -o CMakeFiles/hello_world.dir/Message.cpp.o -c /Users/guodongzhu/SourceCode/CMakeCookbook/chapter01/Message.cpp
[100%] Linking CXX executable hello_world
cd /Users/guodongzhu/SourceCode/CMakeCookbook/build/chapter01 && /usr/local/Cellar/cmake/3.21.2/bin/cmake -E cmake_link_script CMakeFiles/hello_world.dir/link.txt --verbose=1
/usr/bin/clang++  -isysroot /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.15.sdk -Wl,-search_paths_first -Wl,-headerpad_max_install_names CMakeFiles/hello_world.dir/hello_world.cpp.o CMakeFiles/hello_world.dir/Message.cpp.o -o hello_world 
[100%] Built target hello_world
/usr/local/Cellar/cmake/3.21.2/bin/cmake -E cmake_progress_start /Users/guodongzhu/SourceCode/CMakeCookbook/build/CMakeFiles 0
```

查找系统可用的编译器：

```bash
➜  build git:(master) ✗ cmake --system-information information.txt
➜  build git:(master) ✗ grep "CMAKE_CXX_COMPILER_LIST" information.txt 
CMAKE_CXX_COMPILER_LIST "c++;g++;cl;bcc;icpx;icx;clang++"
```

获取cmake编译器信息：

- `CMAKE_CXX_COMPILER_LOADED`
- `CMAKE_CXX_COMPILER_ID`
- `CMAKE_COMPILER_IS_GNUCXX`
- `CMAKE_CXX_COMPILER_VERSION`

切换构建类型：

- `Debug`：用于在没有优化的情况下，使用带有调试符号构建库或可执行文件。
- `Release`：用于构建的优化的库或可执行文件，不包含调试符号。
- `RelWithDebInfo`：用于构建较少的优化库或可执行文件，包含调试符号。
- `MinSizeRel`：用于不增加目标代码大小的优化方式，来构建库或可执行文
件。

```bash
➜  build git:(master) ✗ cmake -DCMAKE_BUILD_TYPE=Debug ..  
-- Build type: Debug
```

设置编译器选项：
