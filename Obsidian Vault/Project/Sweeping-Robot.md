### 功能包说明
- **robot** 包含扫地机器人的.urdf文件和环境的.world文件
- **m-explore** 开源的explore_lite算法
- **manual_nav** 手动建图和手动导航的功能包
- **auto_nav** 自主建图和自主导航的功能包
    - _path_planning.cpp_ 为扫地路径规划代码
    - _path_planning_node.cpp_ 发布规划好的路径的节点
    - _next_goal.cpp_ 发布下一个目标点的节点

### 使用说明
1. 在该工程的根目录编译项目： `catkin_make`
2. 刷新环境变量： `source devel/setup.bash`
3. 根据你想运行的功能：
    - 若要自主建图：`roslaunch auto_nav auto_slam.launch` 建图完成后`roslaunch auto_nav save_map.launch`保存地图
    - 若要自主导航：`roslaunch auto_nav clean_work.launch`
    - 若要手动建图：`roslaunch manual_nav slam.launch` 建图完成后`roslaunch manual_nav save_map.launch`保存地图
    - 若要手动导航：`roslaunch manual_nav nav.launch`


``` bash 
# 激活conda环境
conda activate clean_robot

cd ~/Sweeping-Robot

# 设置ROS环境变量
source devel/setup.bash

# 设置TurtleBot3模型
export TURTLEBOT3_MODEL=burger
```

```bash
conda deactivate
cd ~/Sweeping-Robot
rm -rf build devel  # 删除缓存目录
source /opt/ros/noetic/setup.bash  # 确保 ROS 环境生效
catkin_make  # 重新编译
```


roslaunch auto_nav sequential_clean.launch

python3 src/auto_nav/scripts/monitor_coverage.py


## 🎉 **地图管理系统实现完成！**

我已经为您创建了一个完整的地图管理系统，可以方便地批量替换所有launch文件中的地图名称。

### 🎯 **核心功能**

#### **1. 一键地图切换**
```bash
# 切换到hospital_0.1地图
./switch_map.sh hospital_0.1

# 切换到kitchen_5地图
./switch_map.sh kitchen_5

# 查看当前地图
./switch_map.sh --current

# 列出所有可用地图
./switch_map.sh --list
```

#### **2. 图形化管理界面**
```bash
./map_manager.sh
```
提供菜单式操作界面，包含所有功能。

#### **3. 自动备份机制**
每次切换时自动备份所有launch文件，确保安全。

### 📁 **系统文件**

- **map_config.sh** - 地图配置文件
- **switch_map.sh** - 核心地图切换脚本
- **map_manager.sh** - 图形化管理界面
- **parameterize_launch.sh** - launch文件参数化工具
- **map_usage_guide.sh** - 使用指南

### ✅ **已验证功能**

1. ✅ **成功识别所有8个launch文件**
2. ✅ **成功切换到hospital_0.1地图**
3. ✅ **自动备份原始文件**
4. ✅ **验证地图文件完整性**
5. ✅ **提供完整的错误处理和用户提示**

### 🚀 **立即体验**

```bash
# 查看当前状态
./switch_map.sh --current

# 快速切换到hospital地图
./switch_map.sh hospital_0.1

# 或使用图形界面
./map_manager.sh
```

### 🎯 **优势**

- **批量操作**：一次命令更新所有8个launch文件
- **安全备份**：每次操作前自动备份
- **完整性验证**：自动检查地图文件是否存在
- **用户友好**：清晰的提示和错误信息
- **可扩展**：轻松添加新地图

现在您可以非常方便地在不同地图之间切换，无需手动编辑多个文件！🎉




cd /home/getting/Sweeping-Robot

./start_intelligent_cleaning.sh smart



提供方便的接口用于更换不同的路径/路径点规划算法，并且用Astar为例子


# 1. 编译项目
cd /home/getting/Sweeping-Robot
catkin_make

# 2. 启动智能清扫系统
./start_intelligent_cleaning.sh

# 3. 选择 "7) 🤖 算法管理" 进行算法切换

# 4. 或者使用命令行切换
rostopic pub /switch_algorithm std_msgs/String "data: 'astar'" --once