https://github.com/Getting05/Fast_lio2_ros2_relocation

```bash
   source /opt/ros/foxy/setup.sh
   source /home/getting/livox/livox_ros_driver2/install/setup.sh
   . ./install/setup.bash
```
### 建图功能

```
ros2 launch livox_ros_driver2 msg_MID360_launch.py
ros2 launch fast_lio mapping.launch.py
```
### 重定位功能：
```bash
   ros2 launch livox_ros_driver2 msg_MID360_launch.py
   ros2 launch example.launch.py 
```

1. CHANGE the `map_path` in `example.launch.py` AND `prior_map_path` in `FAST_LIO/config/fast_lio_relocalization_param.yaml` to the path of the map you want to localize in, THEY SHOULD BE THE SAME.
2. Give an initial guess of the pose of the robot in the map by changing `initial_x`, `initial_y` etc. in `example.launch.py`, or give your initial guess in the rviz using `2D Pose Estimation`.