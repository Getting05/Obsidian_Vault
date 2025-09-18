# ROS教程
[Introduction · Autolabor-ROS机器人入门课程《ROS理论与实践》零基础教程](http://www.autolabor.com.cn/book/ROSTutorials/)
https://www.bilibili.com/video/BV1Ci4y1L7ZZ

## HelloWorld
实现过程如下：
1. 先创建一个工作空间；
2. 再创建一个功能包；
3. 编辑源文件；
4. 编辑配置文件；
5. 编译并执行。
### 1. 创建工作空间并初始化
```
cd ROS_learning

mkdir -p demo/src
cd demo
catkin_make
```

**备注：**
```
catkin_make -DPYTHON_EXECUTABLE=/usr/bin/python3
```
这样 CMake 会在 build 目录里记住 /usr/bin/python3 的路径。
避免出现
CMake Error at /opt/ros/noetic/share/catkin/cmake/empy.cmake:30 (message):
  Unable to find either executable 'empy' or Python module 'em'...  try
  installing the package 'python3-empy'
Call Stack (most recent call first):
  /opt/ros/noetic/share/catkin/cmake/all.cmake:164 (include)
  /opt/ros/noetic/share/catkin/cmake/catkinConfig.cmake:20 (include)
  CMakeLists.txt:58 (find_package)
  
-- Configuring incomplete, errors occurred!
See also "/home/getting/ROS_learning/demo/build/CMakeFiles/CMakeOutput.log".
Invoking "cmake" failed
的报错

如果你不想依赖缓存，可以直接在工作空间的 src/CMakeLists.txt 最上面加一行：
```
set(PYTHON_EXECUTABLE "/usr/bin/python3")
```
加在第一行或者 cmake_minimum_required(...) 之后，这样无论在哪台机器、什么环境编译，都会强制用系统 Python。

### 2. 进入 src 创建 ros 包并添加依赖
```
cd src
catkin_create_pkg 自定义ROS包名 roscpp rospy std_msgs
```
eg：
```
cd src
catkin_create_pkg helloworld roscpp rospy std_msgs
```
上述命令，会在工作空间下生成一个功能包，该功能包依赖于 roscpp、rospy 与 std_msgs，其中roscpp是使用C++实现的库，而rospy则是使用python实现的库，std_msgs是标准消息库，创建ROS功能包时，一般都会依赖这三个库实现。

在src目录下会生成：

```
.
├── CMakeLists.txt -> /opt/ros/noetic/share/catkin/cmake/toplevel.cmake
└── helloworld
    ├── CMakeLists.txt
    ├── include
    │   └── helloworld
    ├── package.xml
    └── src
```
注意helloworld目录下也有一个src目录
### 3. C++版实现
#### 1. 进入 ros 包的 **src 目录**编辑源文件
```
cd 自定义的包
```
eg：
```
cd helloworld
```

在该目录下的src目录下新建一个cpp文件，名字任意。一般可以与自定义的包名相同。
这里我们命名为`helloworld_c.cpp`

**C++源码实现**
```
#include "ros/ros.h"

int main(int argc, char *argv[])
{
    //执行 ros 节点初始化
    ros::init(argc,argv,"hello");
    //创建 ros 节点句柄(非必须)
    ros::NodeHandle n;
    //控制台输出 hello world
    ROS_INFO("hello world!");

    return 0;
}
```

#### 2. 编辑 ros 包下的 Cmakelist.txt文件
```
add_executable(步骤3的源文件名
  src/步骤3的源文件名.cpp
)
```
```
target_link_libraries(步骤3的源文件名
  ${catkin_LIBRARIES}
)

```
这里的`步骤3的源文件名`可以自己任意取

eg:
```
add_executable(demo_hello src/helloworld_c.cpp)
```
```
target_link_libraries(demo_hello
${catkin_LIBRARIES}
)

```


#### 3. 进入工作空间目录并编译
```
cd 自定义空间名称
catkin_make
```

这里的`自定义空间名称`即上文的demo


生成 build devel ....

#### 4. 执行
先启动命令行1：

```
roscore
```
再启动命令行2：

```
cd 工作空间
source ./devel/setup.bash
rosrun 包名 C++节点

```
命令行输出: HelloWorld!
eg:
```
(base) getting@PC:~/ROS_learning/demo$ rosrun helloworld demo_hello
[INFO] [1754741713.248288213]: hello world!
```
这里的包名为helloworld，C++节点名为demo_hello

PS:source ~/工作空间/devel/setup.bash可以添加进.bashrc文件，使用上更方便
- 添加方式1: 直接使用 gedit 或 vi 编辑 .bashrc 文件，最后添加该内容
- 添加方式2:echo "source ~/工作空间/devel/setup.bash" >> ~/.bashrc


| 概念                         | 含义                                                      | 例子                                               | 在工程中的位置                                               | 备注                           |
| -------------------------- | ------------------------------------------------------- | ------------------------------------------------ | ----------------------------------------------------- | ---------------------------- |
| **包名（Package Name）**       | ROS 中的功能模块名，等于包含 `package.xml` 和 `CMakeLists.txt` 的文件夹名 | `my_robot_pkg`                                   | `~/catkin_ws/src/my_robot_pkg/`                       | `rosrun` / `roslaunch` 时需要用  |
| **源文件名（Source File Name）** | 要编译的 `.cpp` 源文件                                         | `talker.cpp`                                     | `~/catkin_ws/src/my_robot_pkg/src/talker.cpp`         | 在 `add_executable()` 中指定     |
| **target 名（Target Name）**  | CMake 编译生成的可执行文件名                                       | `talker_node`                                    | 生成位置：`~/catkin_ws/devel/lib/my_robot_pkg/talker_node` | `target_link_libraries()` 要用 |
| **最终可执行文件路径**              | 编译完成后生成文件的位置                                            | `~/catkin_ws/devel/lib/my_robot_pkg/talker_node` | 位于 `devel/lib/<包名>/`                                  | 由 target 名决定                 |
| **运行命令**                   | 运行该节点的 ROS 命令                                           | `rosrun my_robot_pkg talker_node`                | 终端运行                                                  | `包名 + target 名`              |

**一个例子**
```
(base) getting@PC:~$ cd ROS_learning/
(base) getting@PC:~/ROS_learning$  cd demo
(base) getting@PC:~/ROS_learning/demo$ source ./devel/setup.bash
(base) getting@PC:~/ROS_learning/demo$ rosrun helloworld demo_hello
[INFO] [1754743611.490550701]: hello world!
(base) getting@PC:~/ROS_learning/demo$ cd ..
(base) getting@PC:~/ROS_learning$ cd ..
(base) getting@PC:~$ rosrun helloworld demo_hello
[INFO] [1754743617.591364464]: hello world!
```

建议按上面步骤将`source ~/工作空间/devel/setup.bash`添加`~/.bashrc`文件


### 4. Python版实现

#### 1.进入 ros 包添加 scripts 目录并编辑 python 文件

```
cd ros包
mkdir scripts
```

新建 python 文件: (文件名自定义)

```
#! /usr/bin/env python

"""
    Python 版 HelloWorld

"""
import rospy

if __name__ == "__main__":
    rospy.init_node("Hello")
    rospy.loginfo("Hello World!!!!")
```

#### 2.为 python 文件添加可执行权限

```
chmod +x 自定义文件名.py
```

#### 3.编辑 ros 包下的 CamkeList.txt 文件

```
catkin_install_python(PROGRAMS scripts/自定义文件名.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
```
对应创建的python文件
#### 4.进入工作空间目录并编译

```
cd 自定义空间名称
catkin_make
```

#### 5.进入工作空间目录并执行

**先启动命令行1：**

```
roscore
```

**再启动命令行2：**

```
cd 工作空间
source ./devel/setup.bash
rosrun 包名 自定义文件名.py
```

输出结果:`Hello World!!!!`

(ros) getting@PC:~/ROS_learning/demo$ source ./devel/setup.bash
(ros) getting@PC:~/ROS_learning/demo$ rosrun helloworld helloworld_p.py
[INFO] [1755244447.964770]: HelloWorld！

## VSCODE 配置


### 1. 创建 ROS 工作空间

```
mkdir -p xxx_ws/src(必须得有 src)
cd xxx_ws
catkin_make
```

### 2. 启动 vscode

进入 xxx_ws 启动 vscode

```
cd xxx_ws
code .
```

### 3. vscode 中编译 ros

快捷键 ctrl + shift + B 调用编译，选择:`catkin_make:build`

可以点击配置设置为默认，修改.vscode/tasks.json 文件

```
{
// 有关 tasks.json 格式的文档，请参见
    // https://go.microsoft.com/fwlink/?LinkId=733558
    "version": "2.0.0",
    "tasks": [
        {
            "label": "catkin_make:debug", //代表提示的描述性信息
            "type": "shell",  //可以选择shell或者process,如果是shell代码是在shell里面运行一个命令，如果是process代表作为一个进程来运行
            "command": "catkin_make",//这个是我们需要运行的命令
            "args": [],//如果需要在命令后面加一些后缀，可以写在这里，比如-DCATKIN_WHITELIST_PACKAGES=“pac1;pac2”
            "group": {"kind":"build","isDefault":true},
            "presentation": {
                "reveal": "always"//可选always或者silence，代表是否输出信息
            },
            "problemMatcher": "$msCompile"
        }
    ]
}
```

### 4. 创建 ROS 功能包
这里下载的是插件：Robot Developer Extensions for ROS1
选定 src 右击 ---> create catkin package

**设置包名 添加依赖**

### 5. C++ 实现

**在功能包的 src 下新建 cpp 文件**

```
/*
    控制台输出 HelloVSCode !!!

*/
#include "ros/ros.h"

int main(int argc, char *argv[])
{
    setlocale(LC_ALL,"");
    //执行节点初始化
    ros::init(argc,argv,"HelloVSCode");

    //输出日志
    ROS_INFO("Hello VSCode!!!哈哈哈哈哈哈哈哈哈哈");
    return 0;
}
```

**PS1: 如果没有代码提示**

修改 .vscode/c_cpp_properties.json

设置 "cppStandard": "c++17"
我这里是c++14 不用修改

**PS2: main 函数的参数不可以被 const 修饰**

**PS3: 当ROS__INFO 终端输出有中文时，会出现乱码**

[INFO](http://www.autolabor.com.cn/book/ROSTutorials/chapter1/14-ros-ji-cheng-kai-fa-huan-jing-da-jian/142-an-zhuang-vscode.html#): ????????????????????????

解决办法：在函数开头加入下面代码的任意一句

```
setlocale(LC_CTYPE, "zh_CN.utf8");
setlocale(LC_ALL, "");
```

### 6. python 实现

在 功能包 下新建 scripts 文件夹，添加 python 文件，**并添加可执行权限**

```
#! /usr/bin/env python
"""
    Python 版本的 HelloVScode，执行在控制台输出 HelloVScode
    实现:
    1.导包
    2.初始化 ROS 节点
    3.日志输出 HelloWorld


"""

import rospy # 1.导包

if __name__ == "__main__":

    rospy.init_node("Hello_Vscode_p")  # 2.初始化 ROS 节点
    rospy.loginfo("Hello VScode, 我是 Python ....")  #3.日志输出 HelloWorld
```

### 7. 配置 CMakeLists.txt

C++ 配置:

```
add_executable(节点名称
  src/C++源文件名.cpp
)
target_link_libraries(节点名称
  ${catkin_LIBRARIES}
)
```

Python 配置:

```
catkin_install_python(PROGRAMS scripts/自定义文件名.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
```

### 8. 编译执行

编译: ctrl + shift + B

执行: 和之前一致，只是可以在 VScode 中添加终端，首先执行:`source ./devel/setup.bash`


如果不编译直接执行 python 文件，会抛出异常。

1.第一行解释器声明，可以使用绝对路径定位到 python3 的安装路径 #! /usr/bin/python3，但是不建议

2.建议使用 #!/usr/bin/env python 但是会抛出异常 : /usr/bin/env: “python”: 没有那个文件或目录

3.解决1: #!/usr/bin/env python3 直接使用 python3 但存在问题: 不兼容之前的 ROS 相关 python 实现

4.解决2: 创建一个链接符号到 python 命令:`sudo ln -s /usr/bin/python3 /usr/bin/python`


## Launch文件
src/launch/xxxx.launch
实现: 
选定功能包右击 ---> 添加 launch 文件夹

选定 launch 文件夹右击 ---> 添加 launch 文件
以上功能手动创建就好，我所使用的插件没有
编辑 launch 文件内容

```xml
<launch>
    <node pkg="helloworld" type="demo_hello" name="hello" output="screen" />
    <node pkg="turtlesim" type="turtlesim_node" name="t1"/>
    <node pkg="turtlesim" type="turtle_teleop_key" name="key1" />
</launch>
```

node ---> 包含的某个节点
pkg -----> 功能包
type ----> 被运行的节点文件
name --> 为节点命名 自己命名
output-> 设置日志的输出目标，节点中有日志输出要加

运行 launch 文件

roslaunch 包名 launch文件名

运行结果: 一次性启动了多个节点

## ROS 架构
### WorkSpace --- 自定义的工作空间

```
    |--- build:编译空间，用于存放CMake和catkin的缓存信息、配置信息和其他中间文件。

    |--- devel:开发空间，用于存放编译后生成的目标文件，包括头文件、动态&静态链接库、可执行文件等。

    |--- src: 源码

        |-- package：功能包(ROS基本单元)包含多个节点、库与配置文件，包名所有字母小写，只能由字母、数字与下划线组成

            |-- CMakeLists.txt 配置编译规则，比如源文件、依赖项、目标文件

            |-- package.xml 包信息，比如:包名、版本、作者、依赖项...(以前版本是 manifest.xml)

            |-- scripts 存储python文件

            |-- src 存储C++源文件

            |-- include 头文件

            |-- msg 消息通信格式文件

            |-- srv 服务通信格式文件

            |-- action 动作格式文件

            |-- launch 可一次性运行多个节点 

            |-- config 配置信息

        |-- CMakeLists.txt: 编译的基本配置
```

##### 1. package.xml

该文件定义有关软件包的属性，例如软件包名称，版本号，作者，维护者以及对其他catkin软件包的依赖性。请注意，该概念类似于旧版 rosbuild 构建系统中使用的_manifest.xml_文件。

```
​<?xml version="1.0"?>
<!-- 格式: 以前是 1，推荐使用格式 2 -->
<package format="2">
  <!-- 包名 -->
  <name>demo01_hello_vscode</name>
  <!-- 版本 -->
  <version>0.0.0</version>
  <!-- 描述信息 -->
  <description>The demo01_hello_vscode package</description>

  <!-- One maintainer tag required, multiple allowed, one person per tag -->
  <!-- Example:  -->
  <!-- <maintainer email="jane.doe@example.com">Jane Doe</maintainer> -->
  <!-- 维护人员 -->
  <maintainer email="xuzuo@todo.todo">xuzuo</maintainer>


  <!-- One license tag required, multiple allowed, one license per tag -->
  <!-- Commonly used license strings: -->
  <!--   BSD, MIT, Boost Software License, GPLv2, GPLv3, LGPLv2.1, LGPLv3 -->
  <!-- 许可证信息，ROS核心组件默认 BSD -->
  <license>TODO</license>


  <!-- Url tags are optional, but multiple are allowed, one per tag -->
  <!-- Optional attribute type can be: website, bugtracker, or repository -->
  <!-- Example: -->
  <!-- <url type="website">http://wiki.ros.org/demo01_hello_vscode</url> -->


  <!-- Author tags are optional, multiple are allowed, one per tag -->
  <!-- Authors do not have to be maintainers, but could be -->
  <!-- Example: -->
  <!-- <author email="jane.doe@example.com">Jane Doe</author> -->


  <!-- The *depend tags are used to specify dependencies -->
  <!-- Dependencies can be catkin packages or system dependencies -->
  <!-- Examples: -->
  <!-- Use depend as a shortcut for packages that are both build and exec dependencies -->
  <!--   <depend>roscpp</depend> -->
  <!--   Note that this is equivalent to the following: -->
  <!--   <build_depend>roscpp</build_depend> -->
  <!--   <exec_depend>roscpp</exec_depend> -->
  <!-- Use build_depend for packages you need at compile time: -->
  <!--   <build_depend>message_generation</build_depend> -->
  <!-- Use build_export_depend for packages you need in order to build against this package: -->
  <!--   <build_export_depend>message_generation</build_export_depend> -->
  <!-- Use buildtool_depend for build tool packages: -->
  <!--   <buildtool_depend>catkin</buildtool_depend> -->
  <!-- Use exec_depend for packages you need at runtime: -->
  <!--   <exec_depend>message_runtime</exec_depend> -->
  <!-- Use test_depend for packages you need only for testing: -->
  <!--   <test_depend>gtest</test_depend> -->
  <!-- Use doc_depend for packages you need only for building documentation: -->
  <!--   <doc_depend>doxygen</doc_depend> -->
  <!-- 依赖的构建工具，这是必须的 -->
  <buildtool_depend>catkin</buildtool_depend>

  <!-- 指定构建此软件包所需的软件包 -->
  <build_depend>roscpp</build_depend>
  <build_depend>rospy</build_depend>
  <build_depend>std_msgs</build_depend>

  <!-- 指定根据这个包构建库所需要的包 -->
  <build_export_depend>roscpp</build_export_depend>
  <build_export_depend>rospy</build_export_depend>
  <build_export_depend>std_msgs</build_export_depend>

  <!-- 运行该程序包中的代码所需的程序包 -->  
  <exec_depend>roscpp</exec_depend>
  <exec_depend>rospy</exec_depend>
  <exec_depend>std_msgs</exec_depend>


  <!-- The export tag contains other, unspecified, tags -->
  <export>
    <!-- Other tools can request additional information be placed here -->

  </export>
</package>
```

##### 2. CMakelists.txt

文件**CMakeLists.txt**是CMake构建系统的输入，用于构建软件包。任何兼容CMake的软件包都包含一个或多个CMakeLists.txt文件，这些文件描述了如何构建代码以及将代码安装到何处。

```
​cmake_minimum_required(VERSION 3.0.2) #所需 cmake 版本
project(demo01_hello_vscode) #包名称，会被 ${PROJECT_NAME} 的方式调用

## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
#设置构建所需要的软件包
find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  std_msgs
)

## System dependencies are found with CMake's conventions
#默认添加系统依赖
# find_package(Boost REQUIRED COMPONENTS system)


## Uncomment this if the package has a setup.py. This macro ensures
## modules and global scripts declared therein get installed
## See http://ros.org/doc/api/catkin/html/user_guide/setup_dot_py.html
# 启动 python 模块支持
# catkin_python_setup()

################################################
## Declare ROS messages, services and actions ##
## 声明 ROS 消息、服务、动作... ##
################################################

## To declare and build messages, services or actions from within this
## package, follow these steps:
## * Let MSG_DEP_SET be the set of packages whose message types you use in
##   your messages/services/actions (e.g. std_msgs, actionlib_msgs, ...).
## * In the file package.xml:
##   * add a build_depend tag for "message_generation"
##   * add a build_depend and a exec_depend tag for each package in MSG_DEP_SET
##   * If MSG_DEP_SET isn't empty the following dependency has been pulled in
##     but can be declared for certainty nonetheless:
##     * add a exec_depend tag for "message_runtime"
## * In this file (CMakeLists.txt):
##   * add "message_generation" and every package in MSG_DEP_SET to
##     find_package(catkin REQUIRED COMPONENTS ...)
##   * add "message_runtime" and every package in MSG_DEP_SET to
##     catkin_package(CATKIN_DEPENDS ...)
##   * uncomment the add_*_files sections below as needed
##     and list every .msg/.srv/.action file to be processed
##   * uncomment the generate_messages entry below
##   * add every package in MSG_DEP_SET to generate_messages(DEPENDENCIES ...)

## Generate messages in the 'msg' folder
# add_message_files(
#   FILES
#   Message1.msg
#   Message2.msg
# )

## Generate services in the 'srv' folder
# add_service_files(
#   FILES
#   Service1.srv
#   Service2.srv
# )

## Generate actions in the 'action' folder
# add_action_files(
#   FILES
#   Action1.action
#   Action2.action
# )

## Generate added messages and services with any dependencies listed here
# 生成消息、服务时的依赖包
# generate_messages(
#   DEPENDENCIES
#   std_msgs
# )

################################################
## Declare ROS dynamic reconfigure parameters ##
## 声明 ROS 动态参数配置 ##
################################################

## To declare and build dynamic reconfigure parameters within this
## package, follow these steps:
## * In the file package.xml:
##   * add a build_depend and a exec_depend tag for "dynamic_reconfigure"
## * In this file (CMakeLists.txt):
##   * add "dynamic_reconfigure" to
##     find_package(catkin REQUIRED COMPONENTS ...)
##   * uncomment the "generate_dynamic_reconfigure_options" section below
##     and list every .cfg file to be processed

## Generate dynamic reconfigure parameters in the 'cfg' folder
# generate_dynamic_reconfigure_options(
#   cfg/DynReconf1.cfg
#   cfg/DynReconf2.cfg
# )

###################################
## catkin specific configuration ##
## catkin 特定配置##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## INCLUDE_DIRS: uncomment this if your package contains header files
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
# 运行时依赖
catkin_package(
#  INCLUDE_DIRS include
#  LIBRARIES demo01_hello_vscode
#  CATKIN_DEPENDS roscpp rospy std_msgs
#  DEPENDS system_lib
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
# 添加头文件路径，当前程序包的头文件路径位于其他文件路径之前
include_directories(
# include
  ${catkin_INCLUDE_DIRS}
)

## Declare a C++ library
# 声明 C++ 库
# add_library(${PROJECT_NAME}
#   src/${PROJECT_NAME}/demo01_hello_vscode.cpp
# )

## Add cmake target dependencies of the library
## as an example, code may need to be generated before libraries
## either from message generation or dynamic reconfigure
# 添加库的 cmake 目标依赖
# add_dependencies(${PROJECT_NAME} ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

## Declare a C++ executable
## With catkin_make all packages are built within a single CMake context
## The recommended prefix ensures that target names across packages don't collide
# 声明 C++ 可执行文件
add_executable(Hello_VSCode src/Hello_VSCode.cpp)

## Rename C++ executable without prefix
## The above recommended prefix causes long target names, the following renames the
## target back to the shorter version for ease of user use
## e.g. "rosrun someones_pkg node" instead of "rosrun someones_pkg someones_pkg_node"
#重命名c++可执行文件
# set_target_properties(${PROJECT_NAME}_node PROPERTIES OUTPUT_NAME node PREFIX "")

## Add cmake target dependencies of the executable
## same as for the library above
#添加可执行文件的 cmake 目标依赖
add_dependencies(Hello_VSCode ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

## Specify libraries to link a library or executable target against
#指定库、可执行文件的链接库
target_link_libraries(Hello_VSCode
  ${catkin_LIBRARIES}
)

#############
## Install ##
## 安装 ##
#############

# all install targets should use catkin DESTINATION variables
# See http://ros.org/doc/api/catkin/html/adv_user_guide/variables.html

## Mark executable scripts (Python etc.) for installation
## in contrast to setup.py, you can choose the destination
#设置用于安装的可执行脚本
catkin_install_python(PROGRAMS
  scripts/Hi.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

## Mark executables for installation
## See http://docs.ros.org/melodic/api/catkin/html/howto/format1/building_executables.html
# install(TARGETS ${PROJECT_NAME}_node
#   RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark libraries for installation
## See http://docs.ros.org/melodic/api/catkin/html/howto/format1/building_libraries.html
# install(TARGETS ${PROJECT_NAME}
#   ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   RUNTIME DESTINATION ${CATKIN_GLOBAL_BIN_DESTINATION}
# )

## Mark cpp header files for installation
# install(DIRECTORY include/${PROJECT_NAME}/
#   DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
#   FILES_MATCHING PATTERN "*.h"
#   PATTERN ".svn" EXCLUDE
# )

## Mark other files for installation (e.g. launch and bag files, etc.)
# install(FILES
#   # myfile1
#   # myfile2
#   DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
# )

#############
## Testing ##
#############

## Add gtest based cpp test target and link libraries
# catkin_add_gtest(${PROJECT_NAME}-test test/test_demo01_hello_vscode.cpp)
# if(TARGET ${PROJECT_NAME}-test)
#   target_link_libraries(${PROJECT_NAME}-test ${PROJECT_NAME})
# endif()

## Add folders to be run by python nosetests
# catkin_add_nosetests(test)
```


### ROS文件系统相关命令

#### 1. 增

catkin_create_pkg 自定义包名 依赖包 === 创建新的ROS功能包

sudo apt install xxx === 安装 ROS功能包

#### 2. 删

sudo apt purge xxx ==== 删除某个功能包

#### 3. 查

rospack list === 列出所有功能包

rospack find 包名 === 查找某个功能包是否存在，如果存在返回安装路径

roscd 包名 === 进入某个功能包

rosls 包名 === 列出某个包下的文件

apt search xxx === 搜索某个功能包

#### 4. 改

rosed 包名 文件名 === 修改功能包文件

需要安装 vim

**比如:**rosed turtlesim Color.msg

#### 5. 执行

##### 5.1 roscore

**roscore ===** 是 ROS 的系统先决条件节点和程序的集合， 必须运行 roscore 才能使 ROS 节点进行通信。

roscore 将启动:

- ros master
    
- ros 参数服务器
    
- rosout 日志节点
    

用法:

```
roscore
```

或(指定端口号)

```
roscore -p xxxx
```

##### 5.2 rosrun

**rosrun 包名 可执行文件名** === 运行指定的ROS节点

**比如:**`rosrun turtlesim turtlesim_node`

##### 5.3 roslaunch

**roslaunch 包名 launch文件名** === 执行某个包下的 launch 文件

### ROS计算图

#### 1.计算图简介

前面介绍的是ROS文件结构，是磁盘上 ROS 程序的存储结构，是静态的，而 ros 程序运行之后，不同的节点之间是错综复杂的，ROS 中提供了一个实用的工具:rqt_graph。

rqt_graph能够创建一个显示当前系统运行情况的动态图形。ROS 分布式系统中不同进程需要进行数据交互，计算图可以以点对点的网络形式表现数据交互过程。rqt_graph是rqt程序包中的一部分。

#### 2.计算图安装

如果前期把所有的功能包（package）都已经安装完成，则直接在终端窗口中输入

rosrun rqt_graph rqt_graph

如果未安装则在终端（terminal）中输入

```
$ sudo apt install ros-<distro>-rqt
$ sudo apt install ros-<distro>-rqt-common-plugins
```

请使用你的ROS版本名称（比如:kinetic、melodic、Noetic等）来替换掉\<distro\>。

例如当前版本是 Noetic,就在终端窗口中输入

```
$ sudo apt install ros-noetic-rqt
$ sudo apt install ros-noetic-rqt-common-plugins
```



#### 3. 计算图演示

接下来以 ROS 内置的小乌龟案例来演示计算图

首先，按照前面所示，运行案例

然后，启动新终端，键入: rqt_graph 或 rosrun rqt_graph rqt_graph，可以看到类似下图的网络拓扑图，该图可以显示不同节点之间的关系。
![](http://www.autolabor.com.cn/book/ROSTutorials/assets/%E8%AE%A1%E7%AE%97%E5%9B%BE.PNG)


## 通信

ROS 中的基本通信机制主要有如下三种实现策略:

- 话题通信(发布订阅模式)
- 服务通信(请求响应模式)    
- 参数服务器(参数共享模式)

### 话题通信
#### **概念**
以发布订阅的方式实现不同节点之间数据交互的通信模式。
#### **作用**
用于不断更新的、少逻辑处理的数据传输场景。
#### 1. 理论模型

话题通信实现模型是比较复杂的，该模型如下图所示,该模型中涉及到三个角色:

- ROS Master (管理者)
- Talker (发布者)
- Listener (订阅者)

ROS Master 负责**保管 Talker 和 Listener 注册的信息**，并匹配话题相同的 Talker 与 Listener，帮助 Talker 与 Listener 建立连接，连接建立后，Talker 可以发布消息，且发布的消息会被 Listener 订阅。

![](http://www.autolabor.com.cn/book/ROSTutorials/assets/01%E8%AF%9D%E9%A2%98%E9%80%9A%E4%BF%A1%E6%A8%A1%E5%9E%8B.jpg)整个流程由以下步骤实现:

##### 0.Talker注册

Talker启动后，会通过RPC在 ROS Master 中注册自身信息，其中包含所发布消息的话题名称。ROS Master 会将节点的注册信息加入到注册表中。

##### 1.Listener注册

Listener启动后，也会通过RPC在 ROS Master 中注册自身信息，**包含需要订阅消息的话题名**。ROS Master 会将节点的注册信息加入到注册表中。

##### 2.ROS Master实现信息匹配

ROS Master 会根据注册表中的信息匹配Talker 和 Listener，并通过 RPC 向 Listener 发送 Talker 的 RPC 地址信息。

##### 3.Listener向Talker发送请求

Listener 根据接收到的 RPC 地址，通过 RPC 向 Talker 发送连接请求，传输订阅的话**题名称、消息类型以及通信协议(TCP/UDP)**。

##### 4.Talker确认请求

Talker 接收到 Listener 的请求后，也是通过 RPC 向 Listener **确认**连接信息，并发送自身的 TCP 地址信息。

##### 5.Listener与Talker建立连接

Listener 根据步骤4 返回的消息使用 TCP 与 Talker 建立网络连接。

##### 6.Talker向Listener发送消息

连接建立后，Talker 开始向 Listener 发布消息。

> 注意1:上述实现流程中，前五步使用的 RPC协议，最后两步使用的是 TCP 协议
> 
> 注意2: Talker 与 Listener 的启动无先后顺序要求
> 
> 注意3: Talker 与 Listener 都可以有多个
> 
> 注意4: Talker 与 Listener 连接建立后，不再需要 ROS Master。也即，即便关闭ROS Master，Talker 与 Listern 照常通信。

### 2. 实操
#### C++版
##### 1. 发布方
```cpp
/*
    需求: 实现基本的话题通信，一方发布数据，一方接收数据，
         实现的关键点:
         1.发送方
         2.接收方
         3.数据(此处为普通文本)

         PS: 二者需要设置相同的话题


    消息发布方:
        循环发布信息:HelloWorld 后缀数字编号

    实现流程:
        1.包含头文件 
        2.初始化 ROS 节点:命名(唯一)
        3.实例化 ROS 句柄
        4.实例化 发布者 对象
        5.组织被发布的数据，并编写逻辑发布数据

*/
// 1.包含头文件 
#include "ros/ros.h"
#include "std_msgs/String.h" //普通文本类型的消息
#include <sstream>

int main(int argc, char  *argv[])
{   
    //设置编码
    setlocale(LC_ALL,"");

    //2.初始化 ROS 节点:命名(唯一)
    // 参数1和参数2 后期为节点传值会使用
    // 参数3 是节点名称，是一个标识符，需要保证运行后，在 ROS 网络拓扑中唯一
    ros::init(argc,argv,"talker");
    //3.实例化 ROS 句柄
    ros::NodeHandle nh;//该类封装了 ROS 中的一些常用功能

    //4.实例化 发布者 对象
    //泛型: 发布的消息类型
    //参数1: 要发布到的话题
    //参数2: 队列中最大保存的消息数，超出此阀值时，先进的先销毁(时间早的先销毁)
    ros::Publisher pub = nh.advertise<std_msgs::String>("chatter",10);
    // "chatter" 是话题名称，其他节点可以通过订阅这个话题来接收消息
    // 10 是队列大小，表示在发布者和订阅者之间最多缓存10条消息
    // <std_msgs::String> 是模板参数，指定了要发布的消息类型为字符串消息
    // nh 是一个NodeHandle对象，它是ROS节点与ROS系统交互的主要接口
    // advertise 是一个模板方法，用于声明这个节点要发布特定类型的消息

    //5.组织被发布的数据，并编写逻辑发布数据
    //数据(动态组织)
    std_msgs::String msg;
    // msg.data = "你好啊！！！";
    std::string msg_front = "Hello 你好！"; //消息前缀
    int count = 0; //消息计数器

    //逻辑(一秒10次)
    ros::Rate r(10);
    ros::Duration(3.0).sleep(); //休眠3秒，等待订阅者连接
    //节点不死
    while (ros::ok())
    {
        //使用 stringstream 拼接字符串与编号
        std::stringstream ss;
        ss << msg_front << count;
        msg.data = ss.str();
        //发布消息
        pub.publish(msg);
        //加入调试，打印发送的消息
        ROS_INFO("发送的消息:%s",msg.data.c_str());

        //根据前面制定的发送频率自动休眠 休眠时间 = 1/频率；
        r.sleep();
        count++;//循环结束前，让 count 自增
        //暂无应用
        ros::spinOnce();
    }


    return 0;
}
```

##### 2. 订阅方
```cpp
/*
需求: 实现基本的话题通信，一方发布数据，一方接收数据，
实现的关键点:
1.发送方
2.接收方
3.数据(此处为普通文本)

消息订阅方:
订阅话题并打印接收到的消息

实现流程:
1.包含头文件
2.初始化 ROS 节点:命名(唯一)
3.实例化 ROS 句柄
4.实例化 订阅者 对象
5.处理订阅的消息(回调函数)
6.设置循环调用回调函数
*/

// 1.包含头文件
#include "ros/ros.h"
#include "std_msgs/String.h"

  
void doMsg(const std_msgs::String::ConstPtr& msg_p){
ROS_INFO("我听见:%s",msg_p->data.c_str());
// ROS_INFO("我听见:%s",(*msg_p).data.c_str());
}

int main(int argc, char *argv[])
{
setlocale(LC_ALL,"");

//2.初始化 ROS 节点:命名(唯一)
ros::init(argc,argv,"listener");

//3.实例化 ROS 句柄
ros::NodeHandle nh;

//4.实例化 订阅者 对象
ros::Subscriber sub = nh.subscribe<std_msgs::String>("chatter",10,doMsg);
// nh 是NodeHandle对象，负责与ROS系统进行交互
// subscribe 是一个模板方法，用于订阅特定类型的消息
// <std_msgs::String> 是模板参数，指定了要接收的消息类型为字符串消息，必须与发布者的消息类型匹配
// "chatter" 是话题名称，必须与发布者使用的话题名称完全一致
// 10 是队列大小，表示最多缓存10条未处理的消息
// doMsg 是回调函数名，当接收到新消息时会自动调用这个函数


//5.处理订阅的消息(回调函数)
// 6.设置循环调用回调函数
ros::spin();//循环读取接收的数据，并调用回调函数处理
return 0;

}
```

##### 3. 配置 CMakeLists.txt
```
add_executable(talker_c
  src/talker_c.cpp
)
add_executable(listener_c
  src/listener_c.cpp
)

target_link_libraries(talker_c
  ${catkin_LIBRARIES}
)
target_link_libraries(listener_c
  ${catkin_LIBRARIES}
)
```
##### 4. 执行
1.启动 roscore;
2.启动发布节点;
3.启动订阅节点。

##### 5. 注意
补充0:

vscode 中的 main 函数 声明 int main(int argc, char const *argv[]){}，默认生成 argv 被 const 修饰，需要去除该修饰符

补充1:

ros/ros.h No such file or directory .....
检查 CMakeList.txt find_package 出现重复,删除内容少的即可
参考资料:https://answers.ros.org/question/237494/fatal-error-rosrosh-no-such-file-or-directory/

补充2:

find_package 不添加一些包，也可以运行啊， ros.wiki 答案如下
>You may notice that sometimes your project builds fine even if you did not call find_package with all dependencies. This is because catkin combines all your projects into one, so if an earlier project calls find_package, yours is configured with the same values. But forgetting the call means your project can easily break when built in isolation.

补充3:

订阅时，第一条数据丢失
原因: 发送第一条数据时， publisher 还未在 roscore 注册完毕
解决: 注册后，加入休眠 `ros::Duration(3.0).sleep();` 延迟第一条数据的发送

![[2025-08-18 11-54-12 的屏幕截图 1.png]]

![[2025-08-18 11-54-25 的屏幕截图.png]]


#### python版
##### 1.发布方

```
#! /usr/bin/env python
"""
    需求: 实现基本的话题通信，一方发布数据，一方接收数据，
         实现的关键点:
         1.发送方
         2.接收方
         3.数据(此处为普通文本)

         PS: 二者需要设置相同的话题


    消息发布方:
        循环发布信息:HelloWorld 后缀数字编号

    实现流程:
        1.导包 
        2.初始化 ROS 节点:命名(唯一)
        3.实例化 发布者 对象
        4.组织被发布的数据，并编写逻辑发布数据


"""
#1.导包 
import rospy
from std_msgs.msg import String

if __name__ == "__main__":
    #2.初始化 ROS 节点:命名(唯一)
    rospy.init_node("talker_p")
    #3.实例化 发布者 对象
    pub = rospy.Publisher("chatter",String,queue_size=10)
    #4.组织被发布的数据，并编写逻辑发布数据
    msg = String()  #创建 msg 对象
    msg_front = "hello 你好"
    count = 0  #计数器 
    # 设置循环频率
    rate = rospy.Rate(10)
    while not rospy.is_shutdown():

        #拼接字符串
        msg.data = msg_front + str(count)

        pub.publish(msg)
        rate.sleep()
        rospy.loginfo("写出的数据:%s",msg.data)
        count += 1
```

##### 2.订阅方

```
#! /usr/bin/env python
"""
    需求: 实现基本的话题通信，一方发布数据，一方接收数据，
         实现的关键点:
         1.发送方
         2.接收方
         3.数据(此处为普通文本)


    消息订阅方:
        订阅话题并打印接收到的消息

    实现流程:
        1.导包 
        2.初始化 ROS 节点:命名(唯一)
        3.实例化 订阅者 对象
        4.处理订阅的消息(回调函数)
        5.设置循环调用回调函数



"""
#1.导包 
import rospy
from std_msgs.msg import String

def doMsg(msg):
    rospy.loginfo("I heard:%s",msg.data)

if __name__ == "__main__":
    #2.初始化 ROS 节点:命名(唯一)
    rospy.init_node("listener_p")
    #3.实例化 订阅者 对象
    sub = rospy.Subscriber("chatter",String,doMsg,queue_size=10)
    #4.处理订阅的消息(回调函数)
    #5.设置循环调用回调函数
    rospy.spin()
```

##### 3.添加可执行权限

终端下进入 scripts 执行:`chmod +x *.py`

##### 4.配置 CMakeLists.txt

```
catkin_install_python(PROGRAMS
  scripts/talker_p.py
  scripts/listener_p.py
  DES
```


#### 自定义msg
msgs只是简单的文本文件，每行具有字段类型和字段名称，可以使用的字段类型有：
- int8, int16, int32, int64 (或者无符号类型: uint*)
- float32, float64
- string
- time, duration
- other msg files
- variable-length array[] and fixed-length array[C]

ROS中还有一种特殊类型：Header，标头包含时间戳和ROS中常用的坐标帧信息。会经常看到msg文件的第一行具有Header标头。

**流程:**
1. 按照固定格式创建 msg 文件
2. 编辑配置文件
3. 编译生成可以被 Python 或 C++ 调用的中间文件

##### 1.定义msg文件

功能包下新建 msg 目录，添加文件 Person.msg

```
string name
uint16 age
float64 height
```

##### 2.编辑配置文件

**package.xml**中**添加编译依赖与执行依赖**

```
  <build_depend>message_generation</build_depend>
  <exec_depend>message_runtime</exec_depend>
  <!-- 
  exce_depend 以前对应的是 run_depend 现在非法
  -->
```

**CMakeLists.txt**编辑 msg 相关配置

```
find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  std_msgs
  message_generation
)
# 需要加入 message_generation,必须有 std_msgs
```

```
## 配置 msg 源文件
add_message_files(
  FILES
  Person.msg
)
```

```
# 生成消息时依赖于 std_msgs
generate_messages(
  DEPENDENCIES
  std_msgs
)
```

```
#执行时依赖
catkin_package(
#  INCLUDE_DIRS include
#  LIBRARIES demo02_talker_listener
  CATKIN_DEPENDS roscpp rospy std_msgs message_runtime
#  DEPENDS system_lib
)
```

##### 3.编译

**编译后的中间文件查看:**

C++ 需要调用的中间文件(.../工作空间/devel/include/包名/xxx.h)

![](http://www.autolabor.com.cn/book/ROSTutorials/assets/05vscode_%E8%87%AA%E5%AE%9A%E4%B9%89%E6%B6%88%E6%81%AF%E7%9A%84%E4%B8%AD%E9%97%B4%E6%96%87%E4%BB%B6%28C++%29.PNG)

Python 需要调用的中间文件(.../工作空间/devel/lib/python3/dist-packages/包名/msg)

![](http://www.autolabor.com.cn/book/ROSTutorials/assets/06vscode_%E8%87%AA%E5%AE%9A%E4%B9%89%E6%B6%88%E6%81%AF%E7%9A%84%E4%B8%AD%E9%97%B4%E6%96%87%E4%BB%B6%28Python%29.PNG)


#### 话题通信自定义 msg 调用(C++)

##### 0. vscode 配置

为了方便代码提示以及避免误抛异常，需要先配置 vscode，将前面生成的 head 文件路径配置进 c_cpp_properties.json 的 includepath属性:
**"/xxx/yyy工作空间/devel/include/\*\*" //配置 head 文件的路径**
```
{
    "configurations": [
        {
            "browse": {
                "databaseFilename": "",
                "limitSymbolsToIncludedHeaders": true
            },
            "includePath": [
                "/opt/ros/noetic/include/**",
                "/usr/include/**",
                "/xxx/yyy工作空间/devel/include/**" //配置 head 文件的路径 
            ],
            "name": "ROS",
            "intelliSenseMode": "gcc-x64",
            "compilerPath": "/usr/bin/gcc",
            "cStandard": "c11",
            "cppStandard": "c++17"
        }
    ],
    "version": 4
}
```

##### 1. 发布方
**ros::Publisher pub = nh.advertise<demo02_talker_listener::Person>("chatter_person",1000);**
```
/*
    需求: 循环发布人的信息

*/

#include "ros/ros.h"
#include "demo02_talker_listener/Person.h"

int main(int argc, char *argv[])
{
    setlocale(LC_ALL,"");

    //1.初始化 ROS 节点
    ros::init(argc,argv,"talker_person");

    //2.创建 ROS 句柄
    ros::NodeHandle nh;

    //3.创建发布者对象
    ros::Publisher pub = nh.advertise<demo02_talker_listener::Person>("chatter_person",1000);

    //4.组织被发布的消息，编写发布逻辑并发布消息
    demo02_talker_listener::Person p;
    p.name = "sunwukong";
    p.age = 2000;
    p.height = 1.45;

    ros::Rate r(1);
    while (ros::ok())
    {
        pub.publish(p);
        p.age += 1;
        ROS_INFO("我叫:%s,今年%d岁,高%.2f米", p.name.c_str(), p.age, p.height);

        r.sleep();
        ros::spinOnce();
    }



    return 0;
}
```

##### 2. 订阅方
**ros::Subscriber sub = nh.subscribe<demo02_talker_listener::Person>("chatter_person",10,doPerson);**
```
/*
    需求: 订阅人的信息

*/

#include "ros/ros.h"
#include "demo02_talker_listener/Person.h"

void doPerson(const demo02_talker_listener::Person::ConstPtr& person_p){
    ROS_INFO("订阅的人信息:%s, %d, %.2f", person_p->name.c_str(), person_p->age, person_p->height);
}

int main(int argc, char *argv[])
{   
    setlocale(LC_ALL,"");

    //1.初始化 ROS 节点
    ros::init(argc,argv,"listener_person");
    //2.创建 ROS 句柄
    ros::NodeHandle nh;
    //3.创建订阅对象
    ros::Subscriber sub = nh.subscribe<demo02_talker_listener::Person>("chatter_person",10,doPerson);

    //4.回调函数中处理 person

    //5.ros::spin();
    ros::spin();    
    return 0;
}
```

##### 3. 配置 CMakeLists.txt

需要添加 **add_dependencies** 用以设置所依赖的消息相关的中间文件。

```
add_executable(person_talker src/person_talker.cpp)
add_executable(person_listener src/person_listener.cpp)



add_dependencies(person_talker ${PROJECT_NAME}_generate_messages_cpp)
add_dependencies(person_listener ${PROJECT_NAME}_generate_messages_cpp)


target_link_libraries(person_talker
  ${catkin_LIBRARIES}
)
target_link_libraries(person_listener
  ${catkin_LIBRARIES}
)
```
#### 话题通信自定义 msg 调用(Python)

##### 0. vscode配置
为了方便代码提示以及误抛异常，需要先配置 vscode，将前面生成的 python 文件路径配置进 settings.json

```
{
    "python.autoComplete.extraPaths": [
        "/opt/ros/noetic/lib/python3/dist-packages",
        "/xxx/yyy工作空间/devel/lib/python3/dist-packages"
    ]
}
```

##### 1. 发布方

```
#! /usr/bin/env python
"""
    发布方:
        循环发送消息

"""
import rospy
from demo02_talker_listener.msg import Person


if __name__ == "__main__":
    #1.初始化 ROS 节点
    rospy.init_node("talker_person_p")
    #2.创建发布者对象
    pub = rospy.Publisher("chatter_person",Person,queue_size=10)
    #3.组织消息
    p = Person()
    p.name = "葫芦瓦"
    p.age = 18
    p.height = 0.75

    #4.编写消息发布逻辑
    rate = rospy.Rate(1)
    while not rospy.is_shutdown():
        pub.publish(p)  #发布消息
        rate.sleep()  #休眠
        rospy.loginfo("姓名:%s, 年龄:%d, 身高:%.2f",p.name, p.age, p.height)
```

##### 2. 订阅方

```
#! /usr/bin/env python
"""
    订阅方:
        订阅消息

"""
import rospy
from demo02_talker_listener.msg import Person

def doPerson(p):
    rospy.loginfo("接收到的人的信息:%s, %d, %.2f",p.name, p.age, p.height)


if __name__ == "__main__":
    #1.初始化节点
    rospy.init_node("listener_person_p")
    #2.创建订阅者对象
    sub = rospy.Subscriber("chatter_person",Person,doPerson,queue_size=10)
    rospy.spin() #4.循环
```

##### 3. 权限设置

终端下进入 scripts 执行:`chmod +x *.py`

##### 4. 配置 CMakeLists.txt

```
catkin_install_python(PROGRAMS
  scripts/talker_p.py
  scripts/listener_p.py
  scripts/person_talker.py
  scripts/person_listener.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
```