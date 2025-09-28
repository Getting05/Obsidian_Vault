https://github.com/Ericsii/FAST_LIO_ROS2

https://blog.csdn.net/Hbutneymar/article/details/147161479
https://github.com/Getting05/FAST_LIO_ROS2/tree/ros2

## 1. 先决条件

### 1.1 **Ubuntu** 和 **ROS**
**Ubuntu >= 20.04**
apt PCL 和 Eigen **的默认**值足以让 FAST-LIO 正常工作。
ROS >= Foxy（建议使用 ROS-Humble）。
### 1.2. **PCL 和 Eigen**
PCL >= 1.8，遵循 [PCL 安装](https://pointclouds.org/downloads/#linux)。
```
 sudo apt install ros-foxy-pcl-ros
```
Eigen >= 3.3.4，遵循[Eigen安装](http://eigen.tuxfamily.org/index.php?title=Main_Page)。
```
sudo apt install libeigen3-dev
```
### 1.3. **livox_ros_driver2**

按照[安装livox_ros_driver2](https://github.com/Livox-SDK/livox_ros_driver2)进行。

你也可以使用我修改的那个[livox_ros_driver2](https://github.com/Ericsii/livox_ros_driver2/tree/feature/use-standard-unit)

_Remarks:_

- Since the FAST-LIO must support Livox serials LiDAR firstly, so the **livox_ros_driver** must be installed and **sourced** before run any FAST-LIO launch file.
- How to source? The easiest way is add the line to the end of file , where is the directory of the livox ros driver workspace (should be the directory if you completely followed the livox official document).`source $Licox_ros_driver_dir$/devel/setup.bash``~/.bashrc``$Licox_ros_driver_dir$``ws_livox`
## 2. 构建

克隆存储库并 colcon 构建：

```shell
 cd <ros2_ws>/src # cd into a ros2 workspace folder
    git clone https://github.com/Ericsii/FAST_LIO_ROS2.git --recursive
    cd ..
    rosdep install --from-paths src --ignore-src -y
    colcon build --symlink-install
    . ./install/setup.bash # use setup.zsh if use zsh
```
https://github.com/Getting05/FAST_LIO_ROS2.git
**这里可直接拉取我更改后的仓库，避免后续冲突**


- **请记住在构建之前获取livox_ros_driver（遵循 [1.3 livox_ros_driver](https://github.com/PolarisXQ/SCURM_SentryNavigation/tree/master/FAST_LIO#1.3)）**
- 如果要使用 PCL 的自定义版本，请将以下行添加到 ~/.bashrc`export PCL_ROOT={CUSTOM_PCL_PATH}`

这里运行`colcon build --symlink-install`有一个报错:
报错内容
---
```
getting@PC:~/fast_lio2_ws$ colcon build --symlink-install
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
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/preprocess.h:3,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/preprocess.cpp:1:
/home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/preprocess.h:83:105: warning: ‘using uint16_t = uint16_t’ is deprecated: use std::uint16_t instead of pcl::uint16_t [-Wdeprecated-declarations]
                                                                           intensity)(float, time, time)(uint16_t, ring,
                                                                                                         ^
In file included from /usr/include/pcl-1.10/pcl/PCLPointField.h:10:0,
                 from /usr/include/pcl-1.10/pcl/conversions.h:46,
                 from /opt/ros/foxy/include/pcl_conversions/pcl_conversions.h:46,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/preprocess.h:3,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/preprocess.cpp:1:
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
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/preprocess.h:3,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/preprocess.cpp:1:
/home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/preprocess.h:140:6: warning: ‘using uint8_t = uint8_t’ is deprecated: use std::uint8_t instead of pcl::uint8_t [-Wdeprecated-declarations]
     (uint8_t, tag, tag)
      ^
In file included from /usr/include/pcl-1.10/pcl/PCLPointField.h:10:0,
                 from /usr/include/pcl-1.10/pcl/conversions.h:46,
                 from /opt/ros/foxy/include/pcl_conversions/pcl_conversions.h:46,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/preprocess.h:3,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/preprocess.cpp:1:
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
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/preprocess.h:3,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/preprocess.cpp:1:
/home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/preprocess.h:141:6: warning: ‘using uint8_t = uint8_t’ is deprecated: use std::uint8_t instead of pcl::uint8_t [-Wdeprecated-declarations]
     (uint8_t, line, line)
      ^
In file included from /usr/include/pcl-1.10/pcl/PCLPointField.h:10:0,
                 from /usr/include/pcl-1.10/pcl/conversions.h:46,
                 from /opt/ros/foxy/include/pcl_conversions/pcl_conversions.h:46,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/preprocess.h:3,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/preprocess.cpp:1:
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
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/preprocess.h:3,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/preprocess.cpp:1:
/home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/preprocess.h:149:6: warning: ‘using uint8_t = uint8_t’ is deprecated: use std::uint8_t instead of pcl::uint8_t [-Wdeprecated-declarations]
     (uint8_t, tag, tag)
      ^
In file included from /usr/include/pcl-1.10/pcl/PCLPointField.h:10:0,
                 from /usr/include/pcl-1.10/pcl/conversions.h:46,
                 from /opt/ros/foxy/include/pcl_conversions/pcl_conversions.h:46,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/preprocess.h:3,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/preprocess.cpp:1:
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
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/preprocess.h:3,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/preprocess.cpp:1:
/home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/preprocess.h:150:6: warning: ‘using uint8_t = uint8_t’ is deprecated: use std::uint8_t instead of pcl::uint8_t [-Wdeprecated-declarations]
     (uint8_t, line, line)
      ^
In file included from /usr/include/pcl-1.10/pcl/PCLPointField.h:10:0,
                 from /usr/include/pcl-1.10/pcl/conversions.h:46,
                 from /opt/ros/foxy/include/pcl_conversions/pcl_conversions.h:46,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/preprocess.h:3,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/preprocess.cpp:1:
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
                 from /usr/include/pcl-1.10/pcl/register_point_struct.h:56,
                 from /usr/include/pcl-1.10/pcl/point_types.h:44,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/include/common_lib.h:6,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/IMU_Processing.hpp:10,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/laserMapping.cpp:47:
/home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/preprocess.h:83:105: warning: ‘using uint16_t = uint16_t’ is deprecated: use std::uint16_t instead of pcl::uint16_t [-Wdeprecated-declarations]
                                       intensity)(float, time, time)(uint16_t, ring,
                                                                     ^
In file included from /usr/include/pcl-1.10/pcl/point_types.h:42:0,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/include/common_lib.h:6,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/IMU_Processing.hpp:10,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/laserMapping.cpp:47:
/usr/include/pcl-1.10/pcl/pcl_macros.h:94:94: note: declared here
 t [[deprecated("use std::uint16_t instead of pcl::uint16_t")]] = std::uint16_t;
                                                                               ^
In file included from /usr/include/boost/preprocessor/tuple/elem.hpp:23:0,
                 from /usr/include/boost/preprocessor/arithmetic/add.hpp:21,
                 from /usr/include/boost/mpl/aux_/preprocessor/def_params_tail.hpp:66,
                 from /usr/include/boost/mpl/aux_/na_spec.hpp:28,
                 from /usr/include/boost/mpl/not.hpp:20,
                 from /usr/include/boost/mpl/assert.hpp:17,
                 from /usr/include/pcl-1.10/pcl/point_traits.h:48,
                 from /usr/include/pcl-1.10/pcl/register_point_struct.h:56,
                 from /usr/include/pcl-1.10/pcl/point_types.h:44,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/include/common_lib.h:6,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/IMU_Processing.hpp:10,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/laserMapping.cpp:47:
/home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/preprocess.h:140:6: warning: ‘using uint8_t = uint8_t’ is deprecated: use std::uint8_t instead of pcl::uint8_t [-Wdeprecated-declarations]
     (uint8_t, tag, tag)
      ^
In file included from /usr/include/pcl-1.10/pcl/point_types.h:42:0,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/include/common_lib.h:6,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/IMU_Processing.hpp:10,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/laserMapping.cpp:47:
/usr/include/pcl-1.10/pcl/pcl_macros.h:92:90: note: declared here
 t8_t [[deprecated("use std::uint8_t instead of pcl::uint8_t")]] = std::uint8_t;
                                                                               ^
In file included from /usr/include/boost/preprocessor/tuple/elem.hpp:23:0,
                 from /usr/include/boost/preprocessor/arithmetic/add.hpp:21,
                 from /usr/include/boost/mpl/aux_/preprocessor/def_params_tail.hpp:66,
                 from /usr/include/boost/mpl/aux_/na_spec.hpp:28,
                 from /usr/include/boost/mpl/not.hpp:20,
                 from /usr/include/boost/mpl/assert.hpp:17,
                 from /usr/include/pcl-1.10/pcl/point_traits.h:48,
                 from /usr/include/pcl-1.10/pcl/register_point_struct.h:56,
                 from /usr/include/pcl-1.10/pcl/point_types.h:44,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/include/common_lib.h:6,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/IMU_Processing.hpp:10,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/laserMapping.cpp:47:
/home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/preprocess.h:141:6: warning: ‘using uint8_t = uint8_t’ is deprecated: use std::uint8_t instead of pcl::uint8_t [-Wdeprecated-declarations]
     (uint8_t, line, line)
      ^
In file included from /usr/include/pcl-1.10/pcl/point_types.h:42:0,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/include/common_lib.h:6,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/IMU_Processing.hpp:10,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/laserMapping.cpp:47:
/usr/include/pcl-1.10/pcl/pcl_macros.h:92:90: note: declared here
 t8_t [[deprecated("use std::uint8_t instead of pcl::uint8_t")]] = std::uint8_t;
                                                                               ^
In file included from /usr/include/boost/preprocessor/tuple/elem.hpp:23:0,
                 from /usr/include/boost/preprocessor/arithmetic/add.hpp:21,
                 from /usr/include/boost/mpl/aux_/preprocessor/def_params_tail.hpp:66,
                 from /usr/include/boost/mpl/aux_/na_spec.hpp:28,
                 from /usr/include/boost/mpl/not.hpp:20,
                 from /usr/include/boost/mpl/assert.hpp:17,
                 from /usr/include/pcl-1.10/pcl/point_traits.h:48,
                 from /usr/include/pcl-1.10/pcl/register_point_struct.h:56,
                 from /usr/include/pcl-1.10/pcl/point_types.h:44,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/include/common_lib.h:6,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/IMU_Processing.hpp:10,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/laserMapping.cpp:47:
/home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/preprocess.h:149:6: warning: ‘using uint8_t = uint8_t’ is deprecated: use std::uint8_t instead of pcl::uint8_t [-Wdeprecated-declarations]
     (uint8_t, tag, tag)
      ^
In file included from /usr/include/pcl-1.10/pcl/point_types.h:42:0,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/include/common_lib.h:6,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/IMU_Processing.hpp:10,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/laserMapping.cpp:47:
/usr/include/pcl-1.10/pcl/pcl_macros.h:92:90: note: declared here
 t8_t [[deprecated("use std::uint8_t instead of pcl::uint8_t")]] = std::uint8_t;
                                                                               ^
In file included from /usr/include/boost/preprocessor/tuple/elem.hpp:23:0,
                 from /usr/include/boost/preprocessor/arithmetic/add.hpp:21,
                 from /usr/include/boost/mpl/aux_/preprocessor/def_params_tail.hpp:66,
                 from /usr/include/boost/mpl/aux_/na_spec.hpp:28,
                 from /usr/include/boost/mpl/not.hpp:20,
                 from /usr/include/boost/mpl/assert.hpp:17,
                 from /usr/include/pcl-1.10/pcl/point_traits.h:48,
                 from /usr/include/pcl-1.10/pcl/register_point_struct.h:56,
                 from /usr/include/pcl-1.10/pcl/point_types.h:44,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/include/common_lib.h:6,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/IMU_Processing.hpp:10,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/laserMapping.cpp:47:
/home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/preprocess.h:150:6: warning: ‘using uint8_t = uint8_t’ is deprecated: use std::uint8_t instead of pcl::uint8_t [-Wdeprecated-declarations]
     (uint8_t, line, line)
      ^
In file included from /usr/include/pcl-1.10/pcl/point_types.h:42:0,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/include/common_lib.h:6,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/IMU_Processing.hpp:10,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/laserMapping.cpp:47:
/usr/include/pcl-1.10/pcl/pcl_macros.h:92:90: note: declared here
 t8_t [[deprecated("use std::uint8_t instead of pcl::uint8_t")]] = std::uint8_t;
                                                                               ^
In file included from /opt/ros/foxy/include/rclcpp/node_impl.hpp:40:0,
                 from /opt/ros/foxy/include/rclcpp/node.hpp:1224,
                 from /opt/ros/foxy/include/rclcpp/executors/single_threaded_executor.hpp:28,
                 from /opt/ros/foxy/include/rclcpp/executors.hpp:22,
                 from /opt/ros/foxy/include/rclcpp/rclcpp.hpp:146,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/laserMapping.cpp:45:
/opt/ros/foxy/include/rclcpp/create_service.hpp: In instantiation of ‘typename rclcpp::Service<ServiceT>::SharedPtr rclcpp::create_service(std::shared_ptr<rclcpp::node_interfaces::NodeBaseInterface>, std::shared_ptr<rclcpp::node_interfaces::NodeServicesInterface>, const string&, CallbackT&&, const rmw_qos_profile_t&, rclcpp::CallbackGroup::SharedPtr) [with ServiceT = std_srvs::srv::Trigger; CallbackT = std::_Bind<void (LaserMappingNode::*(LaserMappingNode*, std::_Placeholder<1>, std::_Placeholder<2>))(std::shared_ptr<const std_srvs::srv::Trigger_Request_<std::allocator<void> > >, std::shared_ptr<std_srvs::srv::Trigger_Response_<std::allocator<void> > >)>; typename rclcpp::Service<ServiceT>::SharedPtr = std::shared_ptr<rclcpp::Service<std_srvs::srv::Trigger> >; std::__cxx11::string = std::__cxx11::basic_string<char>; rmw_qos_profile_t = rmw_qos_profile_t; rclcpp::CallbackGroup::SharedPtr = std::shared_ptr<rclcpp::CallbackGroup>]’:
/opt/ros/foxy/include/rclcpp/node_impl.hpp:146:53:   required from ‘typename rclcpp::Service<ServiceT>::SharedPtr rclcpp::Node::create_service(const string&, CallbackT&&, const rmw_qos_profile_t&, rclcpp::CallbackGroup::SharedPtr) [with ServiceT = std_srvs::srv::Trigger; CallbackT = std::_Bind<void (LaserMappingNode::*(LaserMappingNode*, std::_Placeholder<1>, std::_Placeholder<2>))(std::shared_ptr<const std_srvs::srv::Trigger_Request_<std::allocator<void> > >, std::shared_ptr<std_srvs::srv::Trigger_Response_<std::allocator<void> > >)>; typename rclcpp::Service<ServiceT>::SharedPtr = std::shared_ptr<rclcpp::Service<std_srvs::srv::Trigger> >; std::__cxx11::string = std::__cxx11::basic_string<char>; rmw_qos_profile_t = rmw_qos_profile_t; rclcpp::CallbackGroup::SharedPtr = std::shared_ptr<rclcpp::CallbackGroup>]’
/home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/laserMapping.cpp:945:181:   required from here
/opt/ros/foxy/include/rclcpp/create_service.hpp:43:3: error: no matching function for call to ‘rclcpp::AnyServiceCallback<std_srvs::srv::Trigger>::set(std::_Bind<void (LaserMappingNode::*(LaserMappingNode*, std::_Placeholder<1>, std::_Placeholder<2>))(std::shared_ptr<const std_srvs::srv::Trigger_Request_<std::allocator<void> > >, std::shared_ptr<std_srvs::srv::Trigger_Response_<std::allocator<void> > >)>)’
   any_service_callback.set(std::forward<CallbackT>(callback));
   ^~~~~~~~~~~~~~~~~~~~
In file included from /opt/ros/foxy/include/rclcpp/service.hpp:28:0,
                 from /opt/ros/foxy/include/rclcpp/callback_group.hpp:25,
                 from /opt/ros/foxy/include/rclcpp/any_executable.hpp:20,
                 from /opt/ros/foxy/include/rclcpp/memory_strategy.hpp:24,
                 from /opt/ros/foxy/include/rclcpp/memory_strategies.hpp:18,
                 from /opt/ros/foxy/include/rclcpp/executor_options.hpp:20,
                 from /opt/ros/foxy/include/rclcpp/executor.hpp:33,
                 from /opt/ros/foxy/include/rclcpp/executors/multi_threaded_executor.hpp:26,
                 from /opt/ros/foxy/include/rclcpp/executors.hpp:21,
                 from /opt/ros/foxy/include/rclcpp/rclcpp.hpp:146,
                 from /home/getting/fast_lio2_ws/src/FAST_LIO_ROS2/src/laserMapping.cpp:45:
/opt/ros/foxy/include/rclcpp/any_service_callback.hpp:67:8: note: candidate: template<class CallbackT, typename std::enable_if<rclcpp::function_traits::same_arguments<CallbackT, std::function<void(std::shared_ptr<std_srvs::srv::Trigger_Request_<std::allocator<void> > >, std::shared_ptr<std_srvs::srv::Trigger_Response_<std::allocator<void> > >)> >::value, void>::type* <anonymous> > void rclcpp::AnyServiceCallback<ServiceT>::set(CallbackT) [with CallbackT = CallbackT; typename std::enable_if<rclcpp::function_traits::same_arguments<CallbackT, std::function<void(std::shared_ptr<typename ServiceT::Request>, std::shared_ptr<typename ServiceT::Response>)> >::value>::type* <anonymous> = <enumerator>; ServiceT = std_srvs::srv::Trigger]
   void set(CallbackT callback)
        ^~~
/opt/ros/foxy/include/rclcpp/any_service_callback.hpp:67:8: note:   template argument deduction/substitution failed:
/opt/ros/foxy/include/rclcpp/any_service_callback.hpp:65:17: error: no type named ‘type’ in ‘struct std::enable_if<false, void>’
     >::type * = nullptr
                 ^~~~~~~
/opt/ros/foxy/include/rclcpp/any_service_callback.hpp:65:17: note: invalid template non-type parameter
/opt/ros/foxy/include/rclcpp/any_service_callback.hpp:81:8: note: candidate: template<class CallbackT, typename std::enable_if<rclcpp::function_traits::same_arguments<CallbackT, std::function<void(std::shared_ptr<rmw_request_id_t>, std::shared_ptr<std_srvs::srv::Trigger_Request_<std::allocator<void> > >, std::shared_ptr<std_srvs::srv::Trigger_Response_<std::allocator<void> > >)> >::value, void>::type* <anonymous> > void rclcpp::AnyServiceCallback<ServiceT>::set(CallbackT) [with CallbackT = CallbackT; typename std::enable_if<rclcpp::function_traits::same_arguments<CallbackT, std::function<void(std::shared_ptr<rmw_request_id_t>, std::shared_ptr<typename ServiceT::Request>, std::shared_ptr<typename ServiceT::Response>)> >::value>::type* <anonymous> = <enumerator>; ServiceT = std_srvs::srv::Trigger]
   void set(CallbackT callback)
        ^~~
/opt/ros/foxy/include/rclcpp/any_service_callback.hpp:81:8: note:   template argument deduction/substitution failed:
/opt/ros/foxy/include/rclcpp/any_service_callback.hpp:79:17: error: no type named ‘type’ in ‘struct std::enable_if<false, void>’
     >::type * = nullptr
                 ^~~~~~~
/opt/ros/foxy/include/rclcpp/any_service_callback.hpp:79:17: note: invalid template non-type parameter
make[2]: *** [CMakeFiles/fastlio_mapping.dir/build.make:63: CMakeFiles/fastlio_mapping.dir/src/laserMapping.cpp.o] Error 1
make[1]: *** [CMakeFiles/Makefile2:124: CMakeFiles/fastlio_mapping.dir/all] Error 2
make: *** [Makefile:141: all] Error 2
---
Failed   <<< fast_lio [17.9s, exited with code 2]

Summary: 0 packages finished [18.0s]
  1 package failed: fast_lio

```

原因：rclcpp::create_service 的回调签名不匹配（ROS 2 Foxy）
需要对laserMapping.cpp进行更改

更改后正常运行



## 3.直接运行
注意：

A. 请确保 IMU 和 LiDAR 是**同步**的，这很重要。

B.警告消息“Failed to find match for field 'time'”表示rosbag文件中缺少每个LiDAR点的时间戳。这对于前向传播和后向传播很重要。

C. 如果外在因素是给出的，我们建议将**extrinsic_est_en**设置为 false。至于外在初始化，请参考我们最近的工作：[**鲁棒实时激光雷达惯性初始化**](https://github.com/hku-mars/LiDAR_IMU_Init)。

### 3.1 运行使用 ros 启动

按照 [Livox-ros-driver2 安装](https://github.com/Livox-SDK/livox_ros_driver2)连接到您的 PC 到 Livox LiDAR，然后

```shell
cd <ros2_ws>
. install/setup.bash # use setup.zsh if use zsh
ros2 launch fast_lio mapping.launch.py config_file:=avia.yaml
```

根据需要将参数更改为 config 目录下的其他 yaml 文件。`config_file`

启动 livox ros 驱动程序。以 MID360 为例ample。

```shell
ros2 launch livox_ros_driver2 msg_MID360_launch.py
```

- 对于 livox 串行，FAST-LIO 仅支持收集的数据，因为只有它的数据结构会产生每个 LiDAR 点的时间戳，这对于运动不失真非常重要。 现在无法生产它。`livox_lidar_msg.launch``livox_ros_driver2/CustomMsg``livox_lidar.launch`
- 如果要更改帧率，请在进行livox_ros_driver包之前，在 [Livox-ros-driver](https://github.com/Livox-SDK/livox_ros_driver2) 的[livox_lidar_msg中](https://github.com/Livox-SDK/livox_ros_driver/blob/master/livox_ros_driver2/launch/livox_lidar_msg.launch)修改 **publish_freq** 参数。

```
cd <ros2_ws>
. install/setup.bash # use setup.zsh if use zsh
ros2 launch fast_lio mapping.launch.py config_file:=mid360.yaml

```

```
ros2 launch livox_ros_driver2 msg_MID360_launch.py
```

### 3.2 对于带有外部 IMU 的 Livox 串行

mapping_avia.launch 在治疗上支持 mid-70、mid-40 或其他 livox 串行激光雷达，但在运行前需要设置一些参数：

编辑以设置以下参数：`config/avia.yaml`

1. 激光雷达点云主题名称：`lid_topic`
2. IMU 主题名称：`imu_topic`
3. 翻译外在：`extrinsic_T`
4. 旋转外在：（仅支持旋转矩阵）`extrinsic_R`

- FAST-LIO 中的外部参数定义为 IMU 体框架（即 IMU 是基础框架）中激光雷达的姿态（位置和旋转矩阵）。它们可以在官方手册中找到。
- FAST-LIO 为 livox LiDAR 生成一个非常简单的软件时间同步，将参数设置为打开。但**只有在外部时间同步确实无法实现时才**打开，因为软件时间同步无法确保准确性。`time_sync_en`

### 3.4 PCD 文件保存

1. Enable `pcd_save.pcd_save_en` in the config file and set the `map_file_path` to the path where the map will be saved.
2. Launch the fastlio2 according to README.
3. Open RQt and switch to `Plugins->Services->Service Caller`. Trigger the service `/map_save`, then the pcd map file will be generated(click call)
在终端输入rqt即可打开rqt

`pcl_viewer scans.pcd` can visualize the point clouds.

_Tips for pcl_viewer:_

- change what to visualize/color by pressing keyboard 1,2,3,4,5 when pcl_viewer is running.

```
    1 is all random
    2 is X values
    3 is Y values
    4 is Z values
    5 is intensity
```

### pcl_viewer使用教程：
安装
```
sudo apt-get install  pcl-tools
```
查看点云的命令如下：
```
pcl_viewer test.pcd
```
快捷键：

按下键盘h或H后，可以查看一些可用的快捷键，如下所示，可以看到，当我们打开一个pcd文件后，终端会输出它的点云个数。
```
> Loading room_scan1.pcd [done, 434 ms : 112586 points]
Available dimensions: x y z
| Help:
-------
          p, P   : switch to a point-based representation
          w, W   : switch to a wireframe-based representation (where available)
          s, S   : switch to a surface-based representation (where available)

          j, J   : take a .PNG snapshot of the current window view
          c, C   : display current camera/window parameters
          f, F   : fly to point mode

          e, E   : exit the interactor
          q, Q   : stop and call VTK's TerminateApp

           +/-   : increment/decrement overall point size
     +/- [+ ALT] : zoom in/out 

          g, G   : display scale grid (on/off) #显示坐标轴
          u, U   : display lookup table (on/off)

    o, O         : switch between perspective/parallel projection (default = perspective)
    r, R [+ ALT] : reset camera [to viewpoint = {0, 0, 0} -> center_{x, y, z}]
    CTRL + s, S  : save camera parameters
    CTRL + r, R  : restore camera parameters

    ALT + s, S   : turn stereo mode on/off
    ALT + f, F   : switch between maximized window mode and original size

          l, L           : list all available geometric and color handlers for the current actor map
    ALT + 0..9 [+ CTRL]  : switch between different geometric handlers (where available)
          0..9 [+ CTRL]  : switch between different color handlers (where available)

    SHIFT + left click   : select a point (start with -use_point_picking)

          x, X   : toggle rubber band selection mode for left mouse button

```
查看点云的坐标
```
pcl_viewer test.pcd -use_point_picking
```

然后按住`shift`选择点，在界面显示界面选择点云之后，会在终端输出点云坐标。
启用xyz轴:
显示xyz轴
```
pcl_viewer test.pcd -ax 5 #5表示轴的放大倍数
```
一次打开多个pcd文件:
不同窗口分别打开pcd文件
```
 pcl_viewer -multiview 1 pig1.pcd pig2.pcd test.pcd
```
同一窗口打开不同pcd文件
```
 pcl_viewer pig1.pcd pig2.pcd test.pcd
```