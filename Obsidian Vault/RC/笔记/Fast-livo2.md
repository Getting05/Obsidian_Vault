https://github.com/hku-mars/FAST-LIVO2/issues/119
https://www.bilibili.com/video/BV1T142197ci/?share_source=copy_web&vd_source=f77407286e8f0ee71b845d76498ada9d
https://github.com/xuankuzcr/LIV_handhold
简达智能：
https://gitee.com/gwmunan

 FAST-LIVO2复现以及MID360、海康摄像头适配:
https://www.coldwindy.cn/index.php/2025/03/28/fast-livo2%e5%a4%8d%e7%8e%b0%e4%bb%a5%e5%8f%8amid360%e3%80%81%e6%b5%b7%e5%ba%b7%e6%91%84%e5%83%8f%e5%a4%b4%e9%80%82%e9%85%8d/


> 如果只提供LiDAR数据，系统会运行纯LO（LiDAR Odometry）模式。如果提供LiDAR和IMU数据，系统会运行LIO（LiDAR-Inertial Odometry）模式。如果同时提供LiDAR、IMU和camera数据，系统则会运行LIVO（LiDAR-Inertial-Visual Odometry）模式。系统会根据输入数据自动选择相应的模式运行。


因为FAST-LIVO2目前依赖的是Livox_ros_driver，而Mid-360依赖Livox_ros_driver2，导致代码段产生冲突，所以有两种解决方案：

1、替换FAST-LIVO2中所有`livox_ros_drvier`代码段为`livox_ros_driver2`。
https://github.com/Livox-SDK/livox_ros_driver2


For ROS (take Noetic as an example):
source /opt/ros/noetic/setup.sh
./build.sh ROS1


```

以下是适配mid360的fast-livo2版本
https://github.com/pcl5/FAST-LIVO2/tree/main

source opt/ros/noetic/setup.sh
### 安装 Sophus

Sophus Installation for the non-templated/double-only version.

```shell
git clone https://github.com/strasdat/Sophus.git
cd Sophus
git checkout a621ff
mkdir build && cd build && cmake ..
make
sudo make install
```
编译错误：`lvalue required as left operand of assignment`
解决方法
把
```
SO2::SO2()
{
  unit_complex_.real() = 1.;
  unit_complex_.imag() = 0.;
}
```
改成
```
SO2::SO2() : unit_complex_(1., 0.) {}
```

这样避免调用 real()/imag() 赋值，不再依赖它们是否返回引用。


workspace文件结构如下：

├── CMakeLists.txt 
├── FAST-LIVO2
├── livox_ros_driver
└── rpg_vikit


### Vikit

Vikit contains camera models, some math and interpolation functions that we need. Vikit is a catkin project, therefore, download it into your catkin workspace source folder.
这里用的是ros1 noetic
要 source opt/ros/noetic/setup.sh
```shell
# Different from the one used in fast-livo1
cd catkin_ws/src
git clone https://github.com/xuankuzcr/rpg_vikit.git 
FAST_LIVO2```

```
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/src
catkin_init_workspace
```
catkin_init_workspace命令把当前目录初始化为一个ROS工作空间。

[2]编译该工作空间。
```
cd ~/catkin_ws
catkin_make
```
这里fast_livo2对 vikit有依赖关系，要放在同个目录下
mkdir -p ~/fast_livo2_ws/src
cd ~/fast_livo2_ws/src/

放在同一个目录下catkin_make

```
source ~/fast_livo2_ws/devel/setup.bash
```


这里我没有进行内外参的标定，用的是[pcl5](https://github.com/pcl5/FAST-LIVO2/tree/main)中的参数
mid360的配置文件：

```
//如果使用了pcl5大佬的fastlivo2可以不用看这一部分新增文件
common:
  img_topic: "/left_camera/image"
  lid_topic: "/livox/lidar"
  imu_topic: "/livox/imu"
  img_en: 1
  lidar_en: 1
  ros_driver_bug_fix: false
 
extrin_calib:
  extrinsic_T: []
  extrinsic_R: []
  Rcl: []
  Pcl: []
 
time_offset:
 imu_time_offset: 0.0
 img_time_offset: 0.1
 exposure_time_init: 0.0
 
preprocess:
 point_filter_num: 1
 filter_size_surf: 0.1
 lidar_type: 1
 scan_line: 4
 blind: 0.8

 
```


vim camera_pinhole.yaml
```

cam_model: Pinhole
cam_width: 1280
cam_height: 1024
scale: 0.5
cam_fx: 1293.56944
cam_fy: 1293.3155
cam_cx: 626.91359
cam_cy: 522.799224
cam_d0: -0.076160
cam_d1: 0.123001
cam_d2: -0.00113
cam_d3: 0.000251

```


source /opt/ros/noetic/setup.sh
source ~/fast_livo2_ws/devel/setup.bash
roslaunch fast_livo mapping_mid360.launch
roslaunch livox_ros_driver2 msg_MID360.launch


遇到如下问题，/cloud_registered ，没有任何数据，rviz上也没有任何结果

