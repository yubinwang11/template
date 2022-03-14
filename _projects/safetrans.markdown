---
layout: page
title: Multi-Robot Safe Transitions
description:  Online Model Predictive  Control for Multi-Agent System
img: /assets/img/flatland/cover.png
importance: 1
category: [robotics] 

---
My work at MARMot @ NUS was releted to evader-puruit game via Multi-Agent Reinforcement Learning with communication. This implementation was on the basis of OpenAI Gym and MPE. The mission of pursuers is to cage the evader via multi-agent colaboration, to ensure the evader cannot escape from cage for enough long time in the bounded world. Please note the policy of evder is not greedy but trained which means the evader is smart enough to plan the best evadision route. The learning framework and algorithm is MAAC from our friends @ USC.

**Code:**
**MPC version** [MPC RoboTrans][mpc]     
**NVF version**  [NVF RoboTrans][nvf]

<div  align="center">    
<iframe height=498 width=510 src="//player.bilibili.com/player.html?aid=632798895&bvid=BV1Wb4y1U7xp&cid=403696803&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
</div>

[mpc]: https://github.com/yubinwang11/RoboForm_mpc
[nvf]: https://github.com/yubinwang11/RoboForm

 <!--more-->
# Tutorials

This technical report aims to server as course project technical report from Yubin Wang and also be tutorials of Turtlebot3 with ROS2 and Python3 for all members at ANCL which is under the supervision of Prof. Fei Chen at Northeastern University.

Please note that this technical report **has not been published as open source file**.
And we are considering to free this project access to public if in convenience.

Author: Yubin Wang, Du Yong


## Hardware

A remote PC (Laptop)            
Several turtlebot3 mobile robots              
OptiTrack or other kinds of motion capture systems like VICON             
A server with motive(API of OptiTrack)        
   
## Software

Several turtlebot3 burgers with Raspberry Pi inserted by Ubuntu18.04 and relevant ros2 packages are necessary. And you may jump to ROBOTIS official tutorials to seek more constructive references since I would overlook too many details which you can get the access via many ways.
 [ROBOTIS e-Manual]https://emanual.robotis.com)                     
 
 **Note** : we select burger as our testbed in this project and you can choose any other kinds of ROS2 standard platform as you need.
 Repeat above procedures if you need set up multiple turtlebot3.


## Bringup

Assuming all our robots and remote PC are connected to the same WIFI at this setup.
[Remote PC] test ssh can work or not:

```
ssh ubuntu@<NETWORK IP of Raspberry PI>
```

###  Modify Host File
 
If this step can't work, you may modify the host file:

```
sudo vim /etc/hosts
```

and add IP of robots haven't been registered:

```
<robot's IP ie. 192.168.1.181> ubuntu
```

Restart ssh and check if it works.


And bringing up singe robot is not a case anymore:

```
ros2 launch turtlebot3_bringup robot.launch.py
```

### Distribute Name to Each Robo

The trouble which makes you down is **Giving a TurtleBot3 a Namespace for Multi-Robot Experiments**.
A blog from a PhD. candidate at ASU helps. [https://zmk5.github.io/general/guide/2019/09/20/ros2-tb3-namespace.html](https://zmk5.github.io/general/guide/2019/09/20/ros2-tb3-namespace.html).

So let's get started. You should connect each robot via ssh and follow below instructions:

#### Step 1: Create a New ros2 Package:

Start by changing into your `src` directory of your workspace that also contains the `turtlebot3` and `utils` packages provided by ROBOTIS.

```
~$ cd ~/turtlebot3_ws/src
~$ ros2 pkg create my_tb3_launcher

```

Now, create two empty directories in the new package:

```
~$ cd ~/turtlebot3_ws/src/my_tb3_launcher
~$ mkdir launch
~$ mkdir param

```

Change into the `launch` directory and create a new bringup launch file.

```
~$ cd launch
~$ touch my_tb3_bringup.launch.py

```

#### Step2: Copy and Modify Contents from the TB3 Bringup Package into Your Package

In the `turtlebot3/turtlebot3_bringup` ros2 package, copy the contents of `robot.launch.py` into the `my_tb3_bringup.launch.py` with the following changes marked as `# comments` in the following code:

```
import os2

from ament_index_python.packages import get_package_share_directory
from launch import LaunchDescription
from launch.actions import DeclareLaunchArgument
from launch.actions import IncludeLaunchDescription
from launch.launch_description_sources import PythonLaunchDescriptionSource
from launch.substitutions import LaunchConfiguration
from launch.substitutions import ThisLaunchFileDir
from launch_ros.actions import Node


def generate_launch_description():
    TURTLEBOT3_MODEL = os.environ['TURTLEBOT3_MODEL']

    usb_port = LaunchConfiguration('usb_port', default='/dev/ttyACM0')

    tb3_param_dir = LaunchConfiguration(
        'tb3_param_dir',
        default=os.path.join(
            get_package_share_directory('my_tb3_launcher'),  # <--- CHANGE THIS!
            'param',
            TURTLEBOT3_MODEL + '.yaml'))

    use_sim_time = LaunchConfiguration('use_sim_time', default='false')

    return LaunchDescription([
        DeclareLaunchArgument(
            'use_sim_time',
            default_value=use_sim_time,
            description='Use simulation (Gazebo) clock if true'),

        DeclareLaunchArgument(
            'usb_port',
            default_value=usb_port,
            description='Connected USB port with OpenCR'),

        DeclareLaunchArgument(
            'tb3_param_dir',
            default_value=tb3_param_dir,
            description='Full path to turtlebot3 parameter file to load'),

        IncludeLaunchDescription(
            PythonLaunchDescriptionSource(
                [ThisLaunchFileDir(), '/turtlebot3_state_publisher.launch.py']),
            launch_arguments={'use_sim_time': use_sim_time}.items(),
        ),

        IncludeLaunchDescription(
            PythonLaunchDescriptionSource([ThisLaunchFileDir(), '/hlds_laser.launch.py']),  <--- CHANGE THIS
            launch_arguments={'port': '/dev/ttyUSB0', 'frame_id': 'base_scan'}.items(),
        ),

        Node(
            package='turtlebot3_node',
            node_executable='turtlebot3_ros',
            node_namespace='tb3_0',  # <------------------- ADD THIS!
            parameters=[tb3_param_dir],
            arguments=['-i', usb_port],
            output='screen'),
    ])


```

Next, copy the file `turtlebot3_state_publisher.launch.py` from the `turtlebot3_bringup/launch` directory into your package’s `launch` directory. Make sure it has the same name! Once complete, make the following changes as marked by the following comments:

```
import os

from ament_index_python.packages import get_package_share_directory
from launch import LaunchDescription
from launch.actions import DeclareLaunchArgument
from launch.substitutions import LaunchConfiguration
from launch_ros.actions import Node


def generate_launch_description():
    TURTLEBOT3_MODEL = os.environ['TURTLEBOT3_MODEL']

    use_sim_time = LaunchConfiguration('use_sim_time', default='false')
    urdf_file_name = 'turtlebot3_' + TURTLEBOT3_MODEL + '.urdf'

    print("urdf_file_name : {}".format(urdf_file_name))

    urdf = os.path.join(
        get_package_share_directory('turtlebot3_description'),
        'urdf',
        urdf_file_name)

    return LaunchDescription([
        DeclareLaunchArgument(
            'use_sim_time',
            default_value='false',
            description='Use simulation (Gazebo) clock if true'),

        Node(
            package='robot_state_publisher',
            node_executable='robot_state_publisher',
            node_name='robot_state_publisher',
            node_namespace='tb3_0',  # <------------------- ADD THIS!
            output='screen',
            parameters=[{'use_sim_time': use_sim_time}],
            arguments=[urdf]),
    ])


```

Finally, copy the file `hlds_laser.launch.py` from the `hls_lfcd_lds_driver` package located in the `launch` directory into your package’s `launch` directory. Again, make sure it has the same name!. Modify the launch file with the following changes marked by the comments below:

```
import os

from ament_index_python.packages import get_package_share_directory
from launch import LaunchDescription
from launch.actions import DeclareLaunchArgument
from launch.actions import LogInfo
from launch.substitutions import LaunchConfiguration
from launch_ros.actions import Node


def generate_launch_description():
    port = LaunchConfiguration('port', default='/dev/ttyUSB0')

    frame_id = LaunchConfiguration('frame_id', default='laser')

    return LaunchDescription([

        DeclareLaunchArgument(
            'port',
            default_value=port,
            description='Specifying usb port to connected lidar'),

        DeclareLaunchArgument(
            'frame_id',
            default_value=frame_id,
            description='Specifying frame_id of lidar. Default frame_id is \'laser\''),

        Node(
            package='hls_lfcd_lds_driver',
            node_executable='hlds_laser_publisher',
            node_name='hlds_laser_publisher',
            node_namespace='tb3_0',  # <------------------- ADD THIS!
            parameters=[{'port': port, 'frame_id': frame_id}],
            output='screen'),
    ])

```

####  Step 3: Modify the Parameter YAML File

Now copy the `burger.yaml` file located in the `param` directory of the `turtlebot3_bringup` package, and make the following modification at the top!

```
tb3_0:
  turtlebot3_node:
    ros__parameters:

      opencr:
        id: 200
        baud_rate: 1000000
        protocol_version: 2.0

      wheels:
        separation: 0.160
        radius: 0.033

      motors:
        profile_acceleration_constant: 214.577

        # [rev/min2]
        # ref) http://emanual.robotis.com/docs/en/dxl/x/xl430-w250/#profile-acceleration
        profile_acceleration: 0.0

      sensors:
        bumper_1: false
        bumper_2: false

        illumination: false

        ir: false

        sonar: false

tb3_0:
  diff_drive_controller:
    ros__parameters:

      odometry:
        publish_tf: true
        use_imu: true
        frame_id: "odom"
        child_frame_id: "base_footprint"

```

As you can see, the top most parameter used to be the node name (`turtlebot3_node` and `diff_drive_controller`). For the node namespace that you added to work, you will need to add the node namespace (`tb3_0`) one level _above_ the node name!

In Step 2, we already changed the launch file to point to _this_ yaml file instead of the one located in the `turtlebot3_bringup` package.

#### Step 4: Modify the CMakeLists File

For this section, we will just be adding a small code snipet to our `CMakeLists.txt` that will install the `launch` and `param` contents of our `my_tb3_launcher` package.

```
...
install(DIRECTORY
  launch
  param
  DESTINATION share/${PROJECT_NAME}/
)
...

```

Add this snippet right before the `if(BUILD_TESTING)` section of the `CMakeLists.txt` file.

#### Step 5: Compile and Run

Finally, compile the code on your TurtleBot3:

```
~$ cd ~/turtlebot3_ws
~$ colcon build --symlink-install --parallel-workers 1
~$ . install/setup.bash

```

Now, run your launch file to make sure it works!

```
~$ export TURTLEBOT3_MODEL=burger
~$ ros2 launch my_tb3_launcher my_tb3_bringup.launch.py

```

You should get the following topics when you run `ros2 topic list` in another bash session:

```
/tb3_0/battery_state
/tb3_0/cmd_vel
/tb3_0/imu
/tb3_0/joint_states
/tb3_0/magnetic_field
/tb3_0/odom
/tb3_0/parameter_events
/tb3_0/robot_description
/tb3_0/rosout
/tb3_0/scan
/tb3_0/sensor_state
/tb3_0/tf
/tb3_0/tf_static

```

You can repeat these procedures with other TurtleBot3 robots with different namespaces to have multiple robots working in your network and then bring up all them.

By the way,  it's accessible for robots with their own namespace to response indicators from remote PC. For example, in terminal on remote PC:

```
 ros2 run turtlebot3_teleop teleop_keyboard --ros -arg --remap /cmd_vel:=/tb3_0/cmd_vel
```

Run this keyboard-based control file to test it.



## OptiTrack

In such multi-robot experiments, OptiTrack or other kinds of global motion capture systems can provided us with reliable state measurements. It's common that OptiTrack communicates with our PC via VRPN, a kind of data streaming. 

### Comms between ROS and OptiTrack

This section is translated from previous CSDN blog ["Optitrack与ROS详细教程以及Motive的使用"](https://blog.csdn.net/weixin_41536025/article/details/89913961).

#### Software Installation

1. Run Motive installaton package      
2. Install USB drive      
3. Install Motive to server (default installation path is OK).       

#### Software Activation  

1. Open Optitrack -> License Tool      
2. Fill in License Serial Number/ License Hash/ Hardware Key Serial Number as your bill information.       
3. Click "Activation".   

#### Calibration

If there is any change on any camera, please recaibration to avoid potential localization inaccuracy.
And calibration accuracy would decrease as time ,varying temperature and other experimental elements, so we recommend that recalibrate cameras every day.       

1. Select "Perform Camera Callibration" in the starting interface.      
2. Clear all masks via camera preview.     
3. Click "Mask Visible" to clear all shinning and unremovable objects in court.       
4. Start Wanding.      
5. Calculae localization errors.     
6. Apply calibration result.      
7. Put the L-shape bar into court. Please make sure constructed coordinate system is correct by cheking the direction of bar.             
8. Set Ground Plane.      

#### Creating Rigidbody

3 ways to create rigidbody:   
1. Create from selected markers after selecting at least three markers.    
2. Use hot key "Ctrl+T" to create it.   
3. Select "New Rigid Body" in Asset window.     

### Tracking Rigidbody 

For rigidbody, the pivot point represents its location and posture. Hence, make sure that the direction of rigidbody's local coordinate system is as same as global coordinate system.     

Rigid Body -> Real-Time Info to view current info of rigidbody.    

#### Data Streaming 

OPen streaming window.

Make sure the following buttons are opened:
1. Broadcast Frame Data.   
2. Broadcast VRPN Data      

#### VRPN Configuration  
```
cd ~/catkin_ws/src
git clone https://github.com/clearpathrobotics/vrpn_client_ros.git
sudo apt-get install ros-kinetic-vrpn # vrpn version should match with your ros version
```
Then,
```
cd ~/catkin_ws
catkin_make
```
Test the communication,
```
ping 192.168.1.2 # ip of server with motive
```

#### Launch VRPN
```
roslaunch vrpn_client_ros sample.launch server:=192.168.1.2 # ip of server with motive
```
### Note 
1. Make sure ip is correct
2. Close server's firewall

###  Bridge between ROS1 and ROS2

It is worth noting that the wide-spread VRPN client, `vrpn_client_ros`, is dependent on ROS1, but turtlebot3 packages are inserted in ROS2. In this case, we introduce a dynamic bridge for which ROS1 and ROS2 can exchange topics, services, messages with each other. 

vrpn_client_ros is a open source package which rely on ROS1 environment, but all our turtlebot package should be run in ROS2. So in this part, we are going to achieve simultaneous ROS1-ROS2 communication.    
For more details,  please refer to [https://github.com/ros2/ros1_bridge](https://github.com/ros2/ros1_bridge).

First we start a ROS 1 roscore:

```
#Shell A (ROS 1 only):
. /opt/ros/melodic/setup.bash
#Or, on OSX, something like:
#. ~/ros_catkin_ws/install_isolated/setup.bash
roscore
```

Then we start the dynamic bridge which will watch the available ROS 1 and ROS 2 topics. Once a matching topic has been detected it starts to bridge the messages on this topic.

```
# Shell B (ROS 1 + ROS 2):
# Source ROS 1 first:
. /opt/ros/melodic/setup.bash
# Or, on OSX, something like:
# . ~/ros_catkin_ws/install_isolated/setup.bash
# Source ROS 2 next:
. <install-space-with-bridge>/setup.bash
# For example:
# . /opt/ros/dashing/setup.bash
export ROS_MASTER_URI=http://localhost:11311
ros2 run ros1_bridge dynamic_bridge

```

The program will start outputting the currently available topics in ROS 1 and ROS 2 in a regular interval.

Now we start the ROS 1 node.

```
# Shell C:
source ~/catkin_ws/devel/setup.bash
# Or, on OSX, something like:
# . ~/ros_catkin_ws/install_isolated/setup.bash
roslaunch vrpn_client_ros sample.launch server:=<IP of server with motive>

```

Basically, you will be shown that vrpn_client_node would be listed after you run `rqt` once you have already launched the bringup file of each robot:

```
ros2 launch my_tb3_launcher my_tb3_bringup.launch.py
```

## Run Our Demo

Congratulations!     
At this step,  believe that you have activated all robotic environments and communication channel.

Don't wait to build workspace:

```
cd ~/turtlebot3_ws
colcon build
```

and run turtlebot3_position_control file:

```
ros2 run turtlebot3_example turtlebot3_position_control
```

