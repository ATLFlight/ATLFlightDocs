# Snapdragon Flight<sup>TM</sup> ROS installation

If you are new to ROS, refer to [ROS Start Guide](http://wiki.ros.org/ROS/StartGuide) first to get started.

1. [Pre-requisites](#pre-requisites)
    * [Platform BSP](#platform-bsp)
1. [Install ROS on Snapdragon Platform](#install-ros-on-snapdragon-platform)
1. [Setup ROS workspace](#setup-ros-workspace)
1. [Setup ROS_IP](#setup-ros_ip)

# Pre-requisites

## Platform BSP

These instructions were tested with version **Flight_3.1.2**. The latest version of the software can be downloaded from [here](http://support.intrinsyc.com/projects/snapdragon-flight/files) and  installed by following the instructions found [here](http://support.intrinsyc.com/projects/snapdragon-flight/wiki)

**NOTE**: By default the HOME environment variable is not set.  Set this up doing the following:

```
adb shell
chmod +rw /home/linaro
echo "export HOME=/home/linaro/" >> /home/linaro/.bashrc
```

If you use SSH, the home environment variable should be set correct after the above step. 
If you use ADB, do the following for each session:

```
adb shell
source /home/linaro/.bashrc
```

# Install ROS on Snapdragon Flight<sup>TM</sup> platform.

This installation step requires the Snapdragon Flight<sup>TM</sup> board to be in **station mode** with internet connection. See instructions [here](http://dev.px4.io/advanced-snapdragon.html#wifi-settings) to set the board in **station mode** and connect it to your WiFi network.

**NOTE** Due to an unmet package dependency for *fastcv-internal* on target, the installation of any deb package using apt-get will fail.  To resolve this, do the following:


```
adb shell
apt-get install -f
```

This will prompt you to uninstall the **fastcv-internal** package.  Type "Y" to uninstall and continue.

## Install the base ROS package on the target

  The following steps are mirrored from [here](http://wiki.ros.org/indigo/Installation/UbuntuARM).  Refer to the link for detailed information about each step.

  1. Set your locale

  ```
  adb shell
  sudo update-locale LANG=C LANGUAGE=C LC_ALL=C LC_MESSAGES=POSIX
  ```

  1. Setup your sources.list

  ```
  adb shell
  sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu trusty main" > /etc/apt/sources.list.d/ros-latest.list'
  ```

  1. Setup your keys

  ```
  adb shell
  sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116
  ```

  1. Installation

  ```
  adb shell
  sudo apt-get update
  sudo apt-get install ros-indigo-ros-base
  ```

  **NOTE**: If the installation fails with this message:

  ```
  E: Failed to fetch http://ports.ubuntu.com/ubuntu-ports/pool/main/i/icu/icu-devtools_52.1-3_armhf.deb  502  cannotconnect [IP: 91.189.88.150 80]
  ```

  try running the following command:

  ```
  apt-get update --fix-missing
  apt-get install ros-indigo-ros-base
  ```

  1. Environment Setup

  ```
  adb shell
  echo "source /opt/ros/indigo/setup.bash" >> /home/linaro/.bashrc
  ```

## Install additional ROS packages on target

It is recommended to install the following additional ROS packages.

  ```
  adb shell
  apt-get install ros-indigo-tf2-ros
  ```

## Initialize ROSDEP
Before you can use ROS, you will need to initialize rosdep. rosdep enables you to easily install system dependencies for source you want to compile and is required to run some core components in ROS.

```
adb shell
sudo rosdep init
rosdep update
```

## Post installation clean-up

After installing ROS, the OpenCL library gets installed by ROS as well which causes a conflict with camera pipeline.  To fix this, do the following:

```
adb shell
dpkg -r libhwloc-plugins
dpkg -r ocl-icd-libopencl1:armhf
```

**NOTE**: OpenCL can be safely removed as there is a version that is installed on the target at /usr/lib.

# Setup ROS workspace

These instructions are mirroed from [here](http://wiki.ros.org/catkin/Tutorials/create_a_workspace).  Refer to the page for additional/detailed information.

  1. Source the ROS environment, if not already done.

  ```
  adb shell
  source /home/linaro/.bashrc
  ```

  1. Create ROS workspace

  ```
  mkdir -p /home/linaro/ros_ws/src
  cd /home/linaro/ros_ws/src
  catkin_init_workspace
  ```

  **NOTE**: The name of the workspace can be anything.  For this demonstration, "ros_ws" is used as the name of the workspace.

  Even though the workspace is empty ( there are no packages in the 'src' folder, just a single CMakeLists.txt link ), you can still build the workspace

  ```
  cd /home/linaro/ros_ws
  catkin_make
  ```

  "catkin_make" is necessary as it creates the /home/linaro/ros_ws/devel directory. This is where the generated libaries and the executables will be generated.  It also generates a new bash setup script which includes the appropriate environment variables for using the "ros_ws" workspace.  It is recommended to add this script to the /home/linaro/.bashrc so that the paths are setup correctly.

  ```
  adb shell
  echo "source /home/linaro/ros_ws/devel/setup.sh" >> /home/linaro/.bashrc
  ```

  That's it you are ready to start developing with ROS.

# Setup ROS_IP

If you plan on using Snapdragon Flight<sup>TM</sup> in softap mode, it is recommended to set the environment variable ROS_IP in the .bashrc file:

```
echo "export ROS_IP=192.168.1.1" >> /home/linaro/.bashrc
```


