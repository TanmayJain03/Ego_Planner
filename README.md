# Instructions for Ego Planner

1. Pull docker image:
```
    docker pull tanmayjain03/xtdrone:latest
```
2. After terminal opens:
```
    tmux
```

Now to operate multiple screens in a terminal (Needed to operate in the same image which opens in a single terminal)
```
    Shift+Ctrl+B+\%     OR   Shift+Ctrl+B+"
```
Host Terminal <br />
Add the below lines to .bashrc of host
```
    export ROS_MASTER_URI=http://172.17.0.2:11311 # Fill in according to the ipconfig result of docker
    export ROS_IP=192.168.1.1
    source /opt/ros/melodic/setup.bash  # melodic/noetic according to your Host version
```
If any errors occur refer ifconfig instructions from this page https://www.yuque.com/xtdrone/manual_en/docker
```
    xhost local:root
```

Terminal 1
```
    cd \~/PX4\_Firmware 
    roslaunch px4 indoor1.launch 
```
Terminal 2
```
    cd ~/XTDrone/communication/
    python multirotor_communication.py iris 0 
```
Terminal 3 (This terminal has commands to control the drone) 
```
    cd ~/XTDrone/control/keyboard 
    python multirotor_keyboard_control.py iris 1 vel  
```
But if you just want to take off and hover 
<br />
    1. Press i till upward velocity in more than 0.3 
    <br /> 
    2. Press b for offboard mode 
    <br />
    3. Press t to takeoff 
    <br />
    4. After reaching sufficient height press s to hover 
    <br />


Terminal 4 (Transforms on camera)
```
    cd ~/XTDrone/motion_planning/3d
    python ego_transfer.py iris 0   
```

Terminal 5 
```
    cd ~/XTDrone/motion_planning/3d 
    rviz -d ego_rviz.rviz  
```
Terminal 6
```
    source ~/ego_ws/devel/setup.bash
    roslaunch ego_planner single_uav.launch 
```

Terminal 7 (Optioinal: Just to observe PCL)
```
    rosrun tf static_transform_publisher 0 0 0 1 -1 1 -1 map  iris_0/depth_camera_base 1000

```

Terminal 8 (To observe all Nodes and Topics)
```
    rqt_graph
```

Terminal 9 (To observe TF links)
```
    rosrun rqt_tf_tree rqt_tf_tree
```
