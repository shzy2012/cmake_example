# CMake

<a href="https://aiden-dong.github.io/2019/07/20/CMake%E6%95%99%E7%A8%8B%E4%B9%8BCMake%E4%BB%8E%E5%85%A5%E9%97%A8%E5%88%B0%E5%BA%94%E7%94%A8/"> CMake</a> 是一个跨平台的、开源的构建工具。cmake 是 makefile 的上层工具，它们的目的正是为了产生可移植的makefile，并简化自己动手写makefile时的巨大工作量.

目前很多开源的项目都可以通过CMake工具来轻松构建工程.

-----


### C/C++ 项目结构
```bash
src : 源码工程目录
ext : 第三方依赖库文件与头文件
CMakeLists.txt : cmake 构建配置文件
```

编译项目
```bash
$ mkdir build
$ cd build/
$ cmake ..         #生成Makefile
$ make             #编译可执行文件
$ ./cmake_example  #执行
```

----
### CMakeLists.txt (CMake 配置文件)

CMake变量
```
用 SET(set) 来定义变量
用 ${NAME} 来获取变量的名称

${CMAKE_MAJOR_VERSION}
```

cmake 编译选项
```bash
编译控制开关名	描述
BUILD_SHARED_LIBS	使用 ADD_LIBRARY 时生成动态库
BUILD_STATIC_LIBS	使用 ADD_LIBRARY 时生成静态库
CMAKE_C_FLAGS	设置 C 编译选项,也可以通过指令 ADD_DEFINITIONS()添加。
CMAKE_CXX_FLAGS	设置 C++编译选项,也可以通过指令 ADD_DEFINITIONS()添加。
```

CMake常用指令
```
1.ADD_DEFINITIONS
语法 : ADD_DEFINITIONS(-DENABLE_DEBUG -DABC)
向 C/C++编译器添加 -D 定义. 如果你的代码中定义了#ifdef ENABLE_DEBUG #endif,这个代码块就会生效。

ADD_DEPENDENCIES
语法: ADD_DEPENDENCIES(target-name depend-target1 depend-target2 ...)
定义 target 依赖的其他 target, 确保在编译本 target 之前,其他的 target 已经被构建。

2.AUX_SOURCE_DIRECTORY
语法 : AUX_SOURCE_DIRECTORY(dir VARIABLE)
作用是发现一个目录下所有的源代码文件并将列表存储在一个变量中,这个指令临时被用来自动构建源文件列表。因为目前 cmake 还不能自动发现新添加的源文件。

3.ADD_SUBDIRECTORY
语法 : ADD_SUBDIRECTORY(NAME) 添加一个文件夹进行编译，该文件夹下的 CMakeLists.txt 负责编译该文件夹下的源码. NAME是想对于调用add_subdirectory的CMakeListst.txt的相对路径．

4.find_package
语法 : find_package(<PackageName> [version] [EXACT] [QUIET] [MODULE] [REQUIRED] [[COMPONENTS] [components...]] [OPTIONAL_COMPONENTS components...] [NO_POLICY_SCOPE])

查找并从外部项目加载设置。 <PackageName>_FOUND 将设置为指示是否找到该软件包。 找到软件包后，将通过软件包本身记录的变量和“导入的目标”提供特定于软件包的信息。 该QUIET选项禁用信息性消息，包括那些如果未找到则表示无法找到软件包的消息REQUIRED。REQUIRED如果找不到软件包，该选项将停止处理并显示一条错误消息。

COMPONENTS选件后（或REQUIRED选件后，如果有的话）可能会列出所需组件的特定于包装的列表 。后面可能会列出其他可选组件OPTIONAL_COMPONENTS。可用组件及其对是否认为找到包的影响由目标包定义。

5.include_directories
语法 : include_directories([AFTER|BEFORE] [SYSTEM] dir1 [dir2 ...])

将给定目录添加到编译器用来搜索包含文件的目录中。相对路径被解释为相对于当前源目录。

包含目录添加到 INCLUDE_DIRECTORIES 当前CMakeLists文件的目录属性。它们也被添加到INCLUDE_DIRECTORIES当前CMakeLists文件中每个目标的target属性。目标属性值是生成器使用的属性值。

6.link_libraries
语法 : link_libraries([item1 [item2 [...]]] [[debug|optimized|general] <item>] ...)

将库链接到以后添加的所有目标。

7.ADD_EXECUTABLE
语法 : ADD_EXECUTABLE(<name> [source1] [source2 ...])

利用源码文件生成目标可执行程序。


8.ADD_LIBRARY
语法 : ADD_LIBRARY(<name> [STATIC | SHARED | MODULE] [source1] [source2 ...])

根据源码文件生成目标库。
STATIC,SHARED 或者 MODULE 可以指定要创建的库的类型。 STATIC库是链接其他目标时使用的目标文件的存档。 SHARED库是动态链接的，并在运行时加载

9.ENABLE_TESTING
语法: ENABLE_TESTING().

控制 Makefile 是否构建 test 目标,涉及工程所有目录。 一般情况这个指令放在工程的主CMakeLists.txt 中.

10.ADD_TEST
语法 : ADD_TEST(testname Exename arg1 arg2 ...)

testname 是自定义的 test 名称,Exename 可以是构建的目标文件也可以是外部脚本等等。 后面连接传递给可执行文件的参数。 如果没有在同一个 CMakeLists.txt 中打开ENABLE_TESTING()指令, 任何 ADD_TEST 都是无效的。

11.CMAKE_MINIMUM_REQUIRED
语法 : CMAKE_MINIMUM_REQUIRED 定义 cmake 的最低兼容版本 比如 CMAKE_MINIMUM_REQUIRED(VERSION 2.5 FATAL_ERROR) 如果 cmake 版本小与 2.5,则出现严重错误,整个过程中止。


12.EXEC_PROGRAM
在 CMakeLists.txt 处理过程中执行命令,并不会在生成的 Makefile 中执行。 具体语法为:

EXEC_PROGRAM(Executable [directory in which to run]
                [ARGS <arguments to executable>]
                [OUTPUT_VARIABLE <var>]
                [RETURN_VALUE <var>])
用于在指定的目录运行某个程序,通过 ARGS 添加参数,如果要获取输出和返回值,可通过OUTPUT_VARIABLE 和 RETURN_VALUE 分别定义两个变量.

这个指令可以帮助你在 CMakeLists.txt 处理过程中支持任何命令,比如根据系统情况去修改代码文件等等。
```


CMake控制指令
```
IF 指令
FOREACH 指令
WHILE 指令
```

---
### 参考
https://aiden-dong.github.io/2019/07/20/CMake教程之CMake从入门到应用/

https://www.hahack.com/codes/cmake/