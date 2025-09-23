https://github.com/PolarisXQ/Fast-LIO2-Localization


```
   source /opt/ros/foxy/setup.sh
   source /home/getting/livox/livox_ros_driver2/install/setup.sh
   . ./install/setup.bash
   ros2 launch livox_ros_driver2 msg_MID360_launch.py
```

ros2 launch fast_lio mapping.launch.py config_file:=mid360.yaml