# Getting Started

These instructions are for using DSPAL and DriverFramework with the Hexagon Toolchain on x86_64 Linux.

## Prerequisites

### Linux Host Setup

These instuctions describe the default installation proceedure for the Hexagon SDK and Tools.
The packages will be installed to ~/Qualcomm/...

The top working dir is assumed to be the user home directory (~), and downloads are assumed to be in
~/Downloads for simplicity.

#### 1. Install clang 3.4.2 or greater, and GCC 4.8 or greater

##### Ubuntu 14.04 to Ubuntu 16.04

Install the system version of clang and gcc:

```
sudo apt-get install clang clang++ gcc g++
```

#### 2. Install CMake 3.4.3 or greater

Do not use cmake 3.4.0. It is known to have issues that break the PX4 build. Later versions (3.4.3+) are fine.

##### CMake

##### Ubuntu 14.04
```
cd ~/Downloads
wget https://cmake.org/files/v3.4/cmake-3.4.3-Linux-x86_64.sh
sudo tar -zxvf cmake-3.4.3-Linux-x86_64.tar.gz -C /opt
```

Add the following to your .bashrc or equivalent for your preferred shell so the path is
updated automatically.

```
if [ "x`echo $PATH | grep '/opt/cmake-3.4.3-Linux-x86_64/bin'`" = "x" ]; then
  PATH=$PATH:/opt/cmake-3.4.3-Linux-x86_64/bin
fi
export PATH=$PATH
```

##### Ubuntu 15.10 to Ubuntu 16.04

Install the system version of cmake:

```
sudo apt-get install cmake
```
#### 3. Hexagon SDK and Hexagon Tools for Linux

Clone the following:
```
git clone https://github.com/ATLFlight/cross_toolchain
cd cross_toolchain
```

Download the [Hexagon SDK 2.0 for Linux](https://developer.qualcomm.com/download/hexagon/hexagon-sdk-linux.bin). You will have to use a browser as it requires QDN registration and a click through.
Request access to "Hexagon.LNX.7.2 Installer-07210.1.tar".

Copy the files to the download dir:
```
cp ~/Downloads/qualcomm_hexagon_sdk_2_0_eval.bin cross_toolchain/downloads
cp ~/Downloads/Hexagon.LNX.7.2\ Installer-07210.1.tar cross_toolchain/downloads

```

To use the Hexagon Tools 7.2.10 you can leave HEXAGON_TOOLS_ROOT unset or set it to:
```
export HEXAGON_TOOLS_ROOT=${HOME}/Qualcomm/HEXAGON_Tools/7.2.10/Tools
```
Now run the install script. This will pop up a GUI that will walk you through the steps of installing the SDK.
NOTE:  Since you are using Hexagon Tools 7.2.10, you can un-check all 3 add-on options (Android NDK, Eclipse, Hexagon Tools) in the installer screen.
```
cd cross_toolchain
./install.sh
```

The script will prompt you to set the following environment variables:
```
export HEXAGON_SDK_ROOT=${HOME}/Qualcomm/Hexagon_SDK/2.0
export HEXAGON_TOOLS_ROOT=${HOME}/Qualcomm/HEXAGON_Tools/7.2.10/Tools
export HEXAGON_ARM_SYSROOT=${HOME}/Qualcomm/Hexagon_SDK/2.0/sysroot
export PATH=${HEXAGON_SDK_ROOT}/gcc-linaro-arm-linux-gnueabihf-4.8-2013.08_linux/bin:$PATH
```

To prevent the path from having multiple versions of the ARM cross compiler path you can do:

```
[ `echo PATH | grep gcc-linaro-arm-linux-gnueabihf-4.8-2013.08_linux/bin`' ] || \
 	export PATH=${HEXAGON_SDK_ROOT}/gcc-linaro-arm-linux-gnueabihf-4.8-2013.08_linux/bin:$PATH
```

## All Done. What Next?

You have installed all the prerequisites to build code for the Hexagon DSP. Try the [HelloWorld](HelloWorld.md)
program to test running a program on the DSP.
