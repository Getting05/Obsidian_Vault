发现两个版本相差很多，于是再运行这个版本看看差别
https://github.com/PolarisXQ/SCURM_SentryNavigation/tree/master/FAST_LIO


```
source /opt/ros/foxy/setup.sh
source /home/getting/livox/livox_ros_driver2/install/setup.sh
. ./install/setup.bash
ros2 launch livox_ros_driver2 msg_MID360_launch.py
ros2 launch fast_lio mapping.launch.py config_file:=mid360.yaml
```

运行构建时报错：
---
```
(base) getting@PC:~/fast_lio_SCURM_ws$     colcon build --symlink-install
Starting >>> fast_lio
--- stderr: fast_lio                             
Current CPU archtecture: x86_64
Processer number:  32
core for MP: 3
CMake Warning (dev) at CMakeLists.txt:81 (find_package):
  Policy CMP0074 is not set: find_package uses <PackageName>_ROOT variables.
  Run "cmake --help-policy CMP0074" for policy details.  Use the cmake_policy
  command to set the policy and suppress this warning.

  CMake variable PCL_ROOT is set to:

    /usr

  For compatibility, CMake is ignoring the variable.
This warning is for project developers.  Use -Wno-dev to suppress it.

Eigen:/usr/include/eigen3
/home/getting/fast_lio_SCURM_ws/src/FAST_LIO/src/laserMapping.cpp:63:10: fatal error: tf2_geometry_msgs/tf2_geometry_msgs.hpp: 没有那个文件或目录
 #include "tf2_geometry_msgs/tf2_geometry_msgs.hpp"
          ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
compilation terminated.
make[2]: *** [CMakeFiles/fastlio_mapping.dir/build.make:63：CMakeFiles/fastlio_mapping.dir/src/laserMapping.cpp.o] 错误 1
make[2]: *** 正在等待未完成的任务....
In file included from /usr/include/boost/preprocessor/tuple/elem.hpp:23:0,
                 from /usr/include/boost/preprocessor/arithmetic/add.hpp:21,
                 from /usr/include/boost/mpl/aux_/preprocessor/def_params_tail.hpp:66,
                 from /usr/include/boost/mpl/aux_/na_spec.hpp:28,
                 from /usr/include/boost/mpl/not.hpp:20,
                 from /usr/include/boost/mpl/assert.hpp:17,
                 from /usr/include/pcl-1.10/pcl/point_traits.h:48,
                 from /usr/include/pcl-1.10/pcl/point_cloud.h:50,
                 from /usr/include/pcl-1.10/pcl/conversions.h:49,
                 from /opt/ros/foxy/include/pcl_conversions/pcl_conversions.h:46,
                 from /home/getting/fast_lio_SCURM_ws/src/FAST_LIO/src/preprocess.h:3,
                 from /home/getting/fast_lio_SCURM_ws/src/FAST_LIO/src/preprocess.cpp:1:
/home/getting/fast_lio_SCURM_ws/src/FAST_LIO/src/preprocess.h:83:105: warning: ‘using uint16_t = uint16_t’ is deprecated: use std::uint16_t instead of pcl::uint16_t [-Wdeprecated-declarations]
                                                                           intensity)(float, time, time)(uint16_t, ring,
                                                                                                         ^
In file included from /usr/include/pcl-1.10/pcl/PCLPointField.h:10:0,
                 from /usr/include/pcl-1.10/pcl/conversions.h:46,
                 from /opt/ros/foxy/include/pcl_conversions/pcl_conversions.h:46,
                 from /home/getting/fast_lio_SCURM_ws/src/FAST_LIO/src/preprocess.h:3,
                 from /home/getting/fast_lio_SCURM_ws/src/FAST_LIO/src/preprocess.cpp:1:
/usr/include/pcl-1.10/pcl/pcl_macros.h:94:94: note: declared here
   using uint16_t [[deprecated("use std::uint16_t instead of pcl::uint16_t")]] = std::uint16_t;
                                                                                              ^
In file included from /usr/include/boost/preprocessor/tuple/elem.hpp:23:0,
                 from /usr/include/boost/preprocessor/arithmetic/add.hpp:21,
                 from /usr/include/boost/mpl/aux_/preprocessor/def_params_tail.hpp:66,
                 from /usr/include/boost/mpl/aux_/na_spec.hpp:28,
                 from /usr/include/boost/mpl/not.hpp:20,
                 from /usr/include/boost/mpl/assert.hpp:17,
                 from /usr/include/pcl-1.10/pcl/point_traits.h:48,
                 from /usr/include/pcl-1.10/pcl/point_cloud.h:50,
                 from /usr/include/pcl-1.10/pcl/conversions.h:49,
                 from /opt/ros/foxy/include/pcl_conversions/pcl_conversions.h:46,
                 from /home/getting/fast_lio_SCURM_ws/src/FAST_LIO/src/preprocess.h:3,
                 from /home/getting/fast_lio_SCURM_ws/src/FAST_LIO/src/preprocess.cpp:1:
/home/getting/fast_lio_SCURM_ws/src/FAST_LIO/src/preprocess.h:131:6: warning: ‘using uint8_t = uint8_t’ is deprecated: use std::uint8_t instead of pcl::uint8_t [-Wdeprecated-declarations]
     (uint8_t, tag, tag)
      ^
In file included from /usr/include/pcl-1.10/pcl/PCLPointField.h:10:0,
                 from /usr/include/pcl-1.10/pcl/conversions.h:46,
                 from /opt/ros/foxy/include/pcl_conversions/pcl_conversions.h:46,
                 from /home/getting/fast_lio_SCURM_ws/src/FAST_LIO/src/preprocess.h:3,
                 from /home/getting/fast_lio_SCURM_ws/src/FAST_LIO/src/preprocess.cpp:1:
/usr/include/pcl-1.10/pcl/pcl_macros.h:92:90: note: declared here
   using uint8_t [[deprecated("use std::uint8_t instead of pcl::uint8_t")]] = std::uint8_t;
                                                                                          ^
In file included from /usr/include/boost/preprocessor/tuple/elem.hpp:23:0,
                 from /usr/include/boost/preprocessor/arithmetic/add.hpp:21,
                 from /usr/include/boost/mpl/aux_/preprocessor/def_params_tail.hpp:66,
                 from /usr/include/boost/mpl/aux_/na_spec.hpp:28,
                 from /usr/include/boost/mpl/not.hpp:20,
                 from /usr/include/boost/mpl/assert.hpp:17,
                 from /usr/include/pcl-1.10/pcl/point_traits.h:48,
                 from /usr/include/pcl-1.10/pcl/point_cloud.h:50,
                 from /usr/include/pcl-1.10/pcl/conversions.h:49,
                 from /opt/ros/foxy/include/pcl_conversions/pcl_conversions.h:46,
                 from /home/getting/fast_lio_SCURM_ws/src/FAST_LIO/src/preprocess.h:3,
                 from /home/getting/fast_lio_SCURM_ws/src/FAST_LIO/src/preprocess.cpp:1:
/home/getting/fast_lio_SCURM_ws/src/FAST_LIO/src/preprocess.h:132:6: warning: ‘using uint8_t = uint8_t’ is deprecated: use std::uint8_t instead of pcl::uint8_t [-Wdeprecated-declarations]
     (uint8_t, line, line)
      ^
In file included from /usr/include/pcl-1.10/pcl/PCLPointField.h:10:0,
                 from /usr/include/pcl-1.10/pcl/conversions.h:46,
                 from /opt/ros/foxy/include/pcl_conversions/pcl_conversions.h:46,
                 from /home/getting/fast_lio_SCURM_ws/src/FAST_LIO/src/preprocess.h:3,
                 from /home/getting/fast_lio_SCURM_ws/src/FAST_LIO/src/preprocess.cpp:1:
/usr/include/pcl-1.10/pcl/pcl_macros.h:92:90: note: declared here
   using uint8_t [[deprecated("use std::uint8_t instead of pcl::uint8_t")]] = std::uint8_t;
                                                                                          ^
make[1]: *** [CMakeFiles/Makefile2:124：CMakeFiles/fastlio_mapping.dir/all] 错误 2
make: *** [Makefile:141：all] 错误 2
---
Failed   <<< fast_lio [7.58s, exited with code 2]

Summary: 0 packages finished [7.71s]
  1 package failed: fast_lio
  1 package had stderr output: fast_lio
```


sudo apt-get install --reinstall ros-foxy-tf2-geometry-msgs

package.xml
```
<?xml version="1.0"?>
<package format="3">
  <name>fast_lio</name>
  <version>0.0.0</version>

  <description>
    This is a modified version of LOAM which is original algorithm
    is described in the following paper:
    J. Zhang and S. Singh. LOAM: Lidar Odometry and Mapping in Real-time.
    Robotics: Science and Systems Conference (RSS). Berkeley, CA, July 2014.
  </description>

  <maintainer email="dev@livoxtech.com">claydergc</maintainer>
  <license>BSD</license>
  <author email="zhangji@cmu.edu">Ji Zhang</author>

  <buildtool_depend>ament_cmake</buildtool_depend>
  <buildtool_depend>rosidl_default_generators</buildtool_depend>

  <depend>geometry_msgs</depend>
  <depend>nav_msgs</depend>
  <depend>rclcpp</depend>
  <depend>std_msgs</depend>
  <depend>sensor_msgs</depend>
  <depend>common_interfaces</depend>
  <depend>tf2</depend>
  <depend>tf2_geometry_msgs</depend>
  <depend>pcl_ros</depend>
  <depend>pcl_conversions</depend>
  <depend>livox_ros_driver2</depend>

  <exec_depend>rosidl_default_runtime</exec_depend>
  <member_of_group>rosidl_interface_packages</member_of_group>

  <export>
    <build_type>ament_cmake</build_type>
  </export>
</package>
```
cmakelist
```
cmake_minimum_required(VERSION 3.8)
project(fast_lio)

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()

ADD_COMPILE_OPTIONS(-std=c++14)
ADD_COMPILE_OPTIONS(-std=c++14)
set(CMAKE_CXX_FLAGS "-std=c++14 -O3")

add_definitions(-DROOT_DIR="${CMAKE_CURRENT_SOURCE_DIR}/")

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fexceptions")
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14 -pthread -std=c++0x -std=c++14 -fexceptions")
set(CMAKE_POSITION_INDEPENDENT_CODE ON)

message("Current CPU archtecture: ${CMAKE_SYSTEM_PROCESSOR}")

if(CMAKE_SYSTEM_PROCESSOR MATCHES "(x86)|(X86)|(amd64)|(AMD64)")
  include(ProcessorCount)
  ProcessorCount(N)
  message("Processer number:  ${N}")

  if(N GREATER 4)
    add_definitions(-DMP_EN)
    add_definitions(-DMP_PROC_NUM=3)
    message("core for MP: 3")
  elseif(N GREATER 3)
    add_definitions(-DMP_EN)
    add_definitions(-DMP_PROC_NUM=2)
    message("core for MP: 2")
  else()
    add_definitions(-DMP_PROC_NUM=1)
  endif()
else()
  add_definitions(-DMP_PROC_NUM=1)
endif()

find_package(OpenMP QUIET)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}   ${OpenMP_C_FLAGS}")

find_package(PythonLibs REQUIRED)
find_path(MATPLOTLIB_CPP_INCLUDE_DIRS "matplotlibcpp.h")

# ROS dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rclcpp_components REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(nav_msgs REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(std_msgs REQUIRED)
find_package(std_srvs REQUIRED)
find_package(visualization_msgs REQUIRED)
find_package(pcl_ros REQUIRED)
find_package(pcl_conversions REQUIRED)
find_package(livox_ros_driver2 REQUIRED)
find_package(rosidl_default_generators REQUIRED)
find_package(tf2 REQUIRED)
find_package(tf2_geometry_msgs REQUIRED)

set(dependencies
  rclcpp
  rclcpp_components
  geometry_msgs
  nav_msgs
  sensor_msgs
  std_msgs
  std_srvs
  visualization_msgs
  pcl_ros
  pcl_conversions
  livox_ros_driver2
  tf2
  tf2_geometry_msgs
)

# Thirdparty libraries
find_package(Eigen3 REQUIRED)
find_package(PCL REQUIRED COMPONENTS common io)

message(Eigen: ${EIGEN3_INCLUDE_DIR})
message(STATUS "PCL: ${PCL_INCLUDE_DIRS}")

set(msg_files
  "msg/Pose6D.msg"
)

rosidl_generate_interfaces(${PROJECT_NAME}
  ${msg_files}
)
ament_export_dependencies(rosidl_default_runtime)

add_executable(fastlio_mapping src/laserMapping.cpp include/ikd-Tree/ikd_Tree.cpp src/preprocess.cpp)
target_include_directories(fastlio_mapping PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
  ${PCL_INCLUDE_DIRS}
)
target_link_libraries(fastlio_mapping ${PCL_LIBRARIES} ${PYTHON_LIBRARIES} Eigen3::Eigen)
target_include_directories(fastlio_mapping PRIVATE ${PYTHON_INCLUDE_DIRS})

list(APPEND EOL_LIST "foxy" "galactic" "eloquent" "dashing" "crystal")

if($ENV{ROS_DISTRO} IN_LIST EOL_LIST)
  # Custommsg to support foxy & galactic
  rosidl_target_interfaces(fastlio_mapping
    ${PROJECT_NAME} "rosidl_typesupport_cpp")
else()
  rosidl_get_typesupport_target(cpp_typesupport_target
    ${PROJECT_NAME} "rosidl_typesupport_cpp")
  target_link_libraries(fastlio_mapping ${cpp_typesupport_target})
endif()

ament_target_dependencies(fastlio_mapping ${dependencies})

# ---------------- Install --------------- #
install(TARGETS fastlio_mapping
  DESTINATION lib/${PROJECT_NAME}
)

install(
  DIRECTORY config launch rviz_cfg
  DESTINATION share/${PROJECT_NAME}
)

ament_package()
```
```
//#include "tf2_geometry_msgs/tf2_geometry_msgs.hpp"

#include "tf2_geometry_msgs/tf2_geometry_msgs.h" 

```



(base) getting@PC:~/fast_lio_SCURM_ws$ ros2 topic list
/Laser_map
/clicked_point
/cloud_effected
/cloud_registered
/cloud_registered_body
/goal_pose
/ikd_tree
/imu/data
/initialpose
/livox/imu
/livox/lidar
/parameter_events
/path
/rosout
/state_estimation
/tf
/tf_static

(base) getting@PC:~/fast_lio_SCURM_ws$ ros2 topic info /livox/lidar
Type: livox_ros_driver2/msg/CustomMsg
Publisher count: 1
Subscription count: 1

(base) getting@PC:~/fast_lio_SCURM_ws$ ros2 topic echo /cloud_registered_body --once
usage: ros2 [-h] Call `ros2 <command> -h` for more detailed usage. ...
ros2: error: unrecognized arguments: --once


(base) getting@PC:~/fast_lio_SCURM_ws$ ros2 topic info /imu/data
Type: sensor_msgs/msg/Imu
Publisher count: 0
Subscription count: 1

(base) getting@PC:~/fast_lio_SCURM_ws$ ros2 topic list
2025-09-21 16:15:07.613 [RTPS_TRANSPORT_SHM Error] Failed init_port fastrtps_port7412: open_and_lock_file failed -> Function open_port_internal
2025-09-21 16:15:07.617 [RTPS_TRANSPORT_SHM Error] Failed init_port fastrtps_port7413: open_and_lock_file failed -> Function open_port_internal
/Laser_map
/clicked_point
/cloud_effected
/cloud_registered
/cloud_registered_body
/goal_pose
/ikd_tree
/initialpose
/livox/imu
/livox/lidar
/parameter_events
/path
/rosout
/state_estimation
/tf
/tf_static


[fastlio_mapping-1] [WARN] [1758442677.213026388] [laserMapping]: "livox_frame" passed to lookupTransform argument target_frame does not exist. 

(base) getting@PC:~/fast_lio_SCURM_ws$ timeout 5s ros2 topic echo /tf
2025-09-21 16:20:10.246 [RTPS_TRANSPORT_SHM Error] Failed init_port fastrtps_port7412: open_and_lock_file failed -> Function open_port_internal
2025-09-21 16:20:10.246 [RTPS_TRANSPORT_SHM Error] Failed init_port fastrtps_port7413: open_and_lock_file failed -> Function open_port_internal
(base) getting@PC:~/fast_lio_SCURM_ws$ ros2 topic hz /tf  # 查看发布频率
2025-09-21 16:20:22.700 [RTPS_TRANSPORT_SHM Error] Failed init_port fastrtps_port7412: open_and_lock_file failed -> Function open_port_internal
2025-09-21 16:20:22.700 [RTPS_TRANSPORT_SHM Error] Failed init_port fastrtps_port7413: open_and_lock_file failed -> Function open_port_internal

(base) getting@PC:~/fast_lio_SCURM_ws$ ros2 topic echo /tf | head -n 1000
2025-09-21 16:20:40.403 [RTPS_TRANSPORT_SHM Error] Failed init_port fastrtps_port7412: open_and_lock_file failed -> Function open_port_internal
2025-09-21 16:20:40.404 [RTPS_TRANSPORT_SHM Error] Failed init_port fastrtps_port7413: open_and_lock_file failed -> Function open_port_internal


^C(base) getting@PC:~/fast_lio_SCURM_ws$ ros2 topic echo /tf
2025-09-21 16:23:09.932 [RTPS_TRANSPORT_SHM Error] Failed init_port fastrtps_port7412: open_and_lock_file failed -> Function open_port_internal
2025-09-21 16:23:09.932 [RTPS_TRANSPORT_SHM Error] Failed init_port fastrtps_port7413: open_and_lock_file failed -> Function open_port_internal




区别在于发布的imu节点是还要经过变换的/imu/data

```
source /home/getting/imu_transfer_ws/install/setup.bash
ros2 run imu_transfer rot_imu
```