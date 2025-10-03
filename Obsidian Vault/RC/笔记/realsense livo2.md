```
# 启动标准RealSense相机节点

roslaunch realsense2_camera rs_camera.launch
```
 roslaunch realsense2_camera rs_camera.launch filters:=pointcloud

## 1. 摄像头标定

### 相机内参标定

代码中已支持多种相机模型，主要需要配置以下参数：

**针孔相机模型 (Pinhole Camera)**：


```
# 在ROS参数中配置
laserMapping/cam_model: "Pinhole"
laserMapping/cam_width: 2448      # 图像宽度
laserMapping/cam_height: 2048     # 图像高度
laserMapping/cam_fx: 2267.341493  # 焦距fx
laserMapping/cam_fy: 2274.135734  # 焦距fy  
laserMapping/cam_cx: 1191.586886  # 主点cx
laserMapping/cam_cy: 1029.989257  # 主点cy
laserMapping/cam_d0: 0.0          # 畸变参数
laserMapping/cam_d1: 0.0
laserMapping/cam_d2: 0.0
laserMapping/cam_d3: 0.0
```

**全向相机模型 (Omni Camera)**：


```
laserMapping/cam_model: "Ocam"
laserMapping/cam_calib_file: "path/to/calibration.txt"
```

### 外参标定

需要标定相机与激光雷达之间的外参：

C++

```
// 在参数文件中配置相机到激光雷达的外参
cameraextrinR: [r11, r12, r13, r21, r22, r23, r31, r32, r33]  // 旋转矩阵
cameraextrinT: [tx, ty, tz]  // 平移向量
```

