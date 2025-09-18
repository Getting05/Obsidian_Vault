### 项目介绍
该项目是一个自主清洁机器人仿真项目，主要基于ROS（机器人操作系统）和TurtleBot3机器人模型实现。项目包含了自主清洁机器人所需的各个模块，如全覆盖路径规划、自主探索建图、导航建图等功能。以下是项目的主要内容：
1. **依赖安装**：需要安装ROS相关的包，如`turtlebot3`、`navigation`、`dwa-local-planner`、`slam-karto`等。
2. **文件结构**：
    - `clean_robot`包：包含路径规划核心程序`CleaningPathPlanner.cpp/h`、发布下一个目标点的源程序`next_goal.cpp`、对路径规划的封装`PathPlanningNode.cpp`等。
    - `Launch文件`：用于启动不同的功能，如清洁工作`clean_work.launch`、自主探索建图`auto_slam.launch`、导航建图`nav_slam.launch`等。
    - `Config`和`param`：分别为路径规划和`move_base`所使用的参数文件。
    - `maps`和`worlds`：分别表示地图文件和仿真环境。
3. **参考资源**：项目参考了多个相关的开源项目，如[CleaningRobot](https://github.com/peterWon/CleaningRobot)、[SLAM-Clean-Robot-Path-Coverage-in-ROS](https://github.com/hjr553199215/SLAM-Clean-Robot-Path-Coverage-in-ROS)等。

### 环境配置（采用新建conda环境的方法）

#### 1. 安装Anaconda
如果你还没有安装Anaconda，可以从[Anaconda官网](https://www.anaconda.com/products/individual)下载适合你操作系统的安装包，并按照安装向导进行安装。

#### 2. 创建并激活conda环境
打开终端，执行以下命令创建一个新的conda环境，并激活该环境：
```bash
conda create -n clean_robot python=3.8
conda activate clean_robot
```

#### 3. 安装ROS
根据你的Ubuntu版本选择合适的ROS版本进行安装。该项目在Ubuntu 18.04+melodic和Ubuntu 20.04+Noetic上测试通过，以下以Ubuntu 20.04+Noetic为例：
```bash
# 设置ROS源
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C65

# 更新系统包列表
sudo apt update

# 安装ROS Noetic
sudo apt install ros-noetic-desktop-full

# 初始化rosdep
sudo rosdep init
rosdep update

# 设置环境变量
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

#### 4. 安装项目依赖
```bash
sudo apt install ros-noetic-turtlebot3 ros-noetic-navigation ros-noetic-dwa-local-planner ros-noetic-slam-karto

pip3 install defusedxml rospkg numpy
``` 
```

#### 5. 下载并编译项目代码
```bash
# 创建工作空间并进入src目录
mkdir -p clean_robot_ws/src && cd clean_robot_ws/src

# 克隆项目代码
git clone https://github.com/li-haojia/Clean-robot-turtlebot3.git

# 回到工作空间根目录
cd ..

# 编译项目
catkin_make
```

#### 6. 设置环境变量
```bash
# 激活conda环境
conda activate clean_robot

# 设置ROS环境变量
source devel/setup.bash

# 设置TurtleBot3模型
export TURTLEBOT3_MODEL=burger
```

### 项目使用方法

#### 1. 自主清扫
```bash
cd clean_robot_ws
source devel/setup.bash
export TURTLEBOT3_MODEL=burger
roslaunch clean_robot clean_work.launch
# 如果想要加载自己的地图，用下面这个。 map_file:="你的地图的绝对路径，加载yaml文件。"
roslaunch clean_robot clean_work.launch map_file:="/home/xxx/clean_robot_ws/homemap.yaml"
```

#### 2. 自主探索
```bash
cd ~/clean_robot_ws
source devel/setup.bash
export TURTLEBOT3_MODEL=burger
roslaunch clean_robot auto_slam.launch
```
保存地图，打开一个新的终端，原来运行的不要关闭：
```bash
rosrun map_server map_saver -f ~/clean_robot_ws/homemap
```

#### 3. 手动导航建图
```bash
cd clean_robot_ws
source devel/setup.bash
export TURTLEBOT3_MODEL=burger
roslaunch clean_robot nav_slam.launch
```
保存地图，打开一个新的终端，原来运行的不要关闭：
```bash
rosrun map_server map_saver -f ~/clean_robot_ws/homemap
电气```

通过以上步骤，你就可以完成项目的环境配置，并使用项目的各个功能了。

```xml
<link name="link">
    <collision name="collision">
      <geometry>
        <box>
          <!-- 缩小尺寸 -->
          <scale>0.5 0.5 0.5</scale>
        </box>
      </geometry>
    </collision>
    <visual name="visual">
      <geometry>
        <box>
          <!-- 缩小尺寸 -->
          <scale>0.5 0.5 0.5</scale>
        </box>
      </geometry>
    </visual>
```