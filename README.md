# Net F/T Sensor Driver

This is meta-package that contains ROS2 software for reading the data from F/T sensors
with RDT communication interface such as: ATI F/T sensors, OnRobot F/T sensors.

- `net_ft_driver`: A ros2_control hardware interface for F/T sensor.
- `net_ft_diagnostic_broadcaster`: A ros2_controller for broadcasting a diagnostic
  data from the F/T sensor.
- `net_ft_description`: A package containing F/T sensors' description files.

Currently this package supports only following F/T sensors:

- OnRobot HEX series
- ATI AXIA series
- ATI Net F/T series

Software was tested with `ATI AXIA80` and `OnRobot HEX-E V2` and `ATI Net F/T series`.

## Installation

The following instructions assume that a [ROS2 Workspace](https://docs.ros.org/en/humble/Tutorials/Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.html) has been created and  you are in its **src** folder.

To install the package execute the following instructions:

```Bash
sudo apt update
sudo apt dist-upgrade
rosdep update
git clone https://github.com/AleBarte/ros2_net_ft_driver.git
sudo apt install -y libasio-dev libcurlpp-dev
cd ..
rosdep install --ignore-src --from-paths src -y -r --rosdistro $ROS_DISTRO
```

Build the package:

```Bash
colcon build --symlink-install
source install/local_setup.sh
```

## Running

Launch the F/T Sensor as a Standalone (useful for testing):

```Bash
ros2 launch net_ft_driver net_ft_broadcaster.launch.py ip_address:=192.168.0.14 sensor_type:=ati rdt_sampling_rate:=500
```

Launch the sensor under a namespace:

```Bash
ros2 launch net_ft_driver net_ft_broadcaster.launch.py ip_address:=192.168.0.14 sensor_type:=ati rdt_sampling_rate:=500 namespace:=sensor
```
This is needed to make the sensor work with real robots, otherwise a crash happens.

where:

- `ip_address`: the IP address of the F/T sensor.
- `sensor_type`: the sensor type, select one of `ati`, `ati_axia`, `onrobot`.
- `rdt_sampling_rate`: the sampling rate of the RDT communication, please refer to
  the sensor manuals for the frequency range.
- `use_hardware_biasing`: whether to use built-in sensor biasing.


>[!IMPORTANT]
>IP addresses which can be used are found on the NET FT Boxes. Make sure to use the one matching your current setup.


>[!NOTE]
>To change `rdt_sampling_rate` the argument in the launch is NOT working. You have to change the `update_rate` in the ros param of the controller manager in the folder `net_ft_driver\config` 