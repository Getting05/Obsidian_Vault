通过 `sync_node.cpp` 实现 LiDAR 和相机的数据同步

### 1. 基于 ROS message_filters 的时间同步


```C++

typedef sync_policies::ApproximateTime<Image, CustomMsg> MySyncPolicy;
Synchronizer<MySyncPolicy> sync;
```

项目使用 ROS 的 `message_filters` 库中的 `ApproximateTime` 策略来实现近似时间同步。这种方法允许在时间戳不完全匹配的情况下，找到时间上最接近的图像和 LiDAR 数据对。

### 2. 双重同步策略

该项目实现了两层同步机制：

**第一层 - 消息同步回调：**


```C++

void callback(const ImageConstPtr& img_msg, const CustomMsgConstPtr& lidar_msg) {
    // Store the latest synchronized messages
    last_image_msg = *img_msg;
    last_lidar_msg = *lidar_msg;
    new_image_received = true;
    new_lidar_received = true;
}
```

当 `message_filters` 找到时间匹配的图像和 LiDAR 数据时，会触发这个回调函数，存储最新的同步消息。

**第二层 - 定时发布机制：**


```C++

void timerCallback(const ros::TimerEvent& event) {
    if (new_image_received && new_lidar_received) {
        image_pub.publish(last_image_msg);
        lidar_pub.publish(last_lidar_msg);
        new_image_received = false;
        new_lidar_received = false;
    }
}
```

通过定时器按照指定频率（`publish_rate`）发布同步后的数据，确保输出的稳定性。

### 3. 配置参数化

系统通过 ROS 参数服务器获取配置：

- `image_topic`: 输入图像话题
- `lidar_topic`: 输入 LiDAR 话题
- `synced_image_topic`: 同步后的图像话题
- `synced_lidar_topic`: 同步后的 LiDAR 话题
- `publish_rate`: 发布频率

### 4. LiDAR 时间戳同步支持

从搜索结果可以看出，项目还支持多种时间戳同步模式：


```C++

typedef enum {
  kTimestampTypeNoSync = 0,     // 无同步信号模式
  kTimestampTypeGptpOrPtp = 1,  // gPTP 或 PTP 同步模式
  kTimestampTypeGps = 2         // GPS 同步模式
}
```

## 工作流程

1. **订阅数据源**：分别订阅相机图像和 LiDAR 点云数据
2. **时间匹配**：使用 `ApproximateTime` 策略找到时间戳接近的数据对
3. **数据缓存**：将匹配的数据对缓存到内部变量
4. **定时发布**：按照设定频率发布同步后的数据

Launch 文件配置示例

项目提供了具体的 launch 文件来设置这些参数：

launch_ROS1/msg_MID360_sync.launch

```
<launch>
  <!-- Start the sync_node -->
  <node name="sync_node" pkg="livox_ros_driver2" type="sync_node" output="screen">
    <param name="image_topic" value="/camera/color/image_raw"/>
    <param name="lidar_topic" value="/livox/lidar"/>
    <param name="synced_image_topic" value
```

这个文件替换原有的msg_MID360_launch.launch
在这个文件中配置话题
### 各参数详细说明

| 参数名称                 | 数据类型   | 功能描述                    | 示例值                                                              |
| -------------------- | ------ | ----------------------- | ---------------------------------------------------------------- |
| `image_topic`        | string | 输入的相机图像话题名称，支持原始图像和压缩图像 | `/camera/color/image_raw` 或 `/camera/color/image_raw/compressed` |
| `lidar_topic`        | string | 输入的LiDAR点云话题名称          | `/livox/lidar`                                                   |
| `synced_image_topic` | string | 同步后输出的图像话题名称            | `/synced_image`                                                  |
| `synced_lidar_topic` | string | 同步后输出的LiDAR话题名称         | `/synced_lidar`                                                  |
| `publish_rate`       | double | 同步数据的发布频率(Hz)           | `10.0`                                                           |



```
<!-- 1. 启动Livox雷达驱动 -->

<include file="$(find livox_ros_driver2)/launch_ROS1/msg_MID360.launch" />

<!-- 2. 启动camera -->



<!-- 3. 启动时间同步节点 -->

<include file="$(find livox_ros_driver2)/launch_ROS1/msg_MID360_sync.launch" />
```




使用https://github.com/hku-mars/livox_camera_calib进行标定

