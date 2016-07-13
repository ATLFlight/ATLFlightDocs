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
chmod a+x cmake-3.4.3-Linux-x86_64.sh
./cmake-3.4.3-Linux-x86_64.sh
sudo mv cmake-3.4.3-Linux-x86_64 /opt
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

#### 3. Install other packages

```
sudo apt-get install python-empy
sudo apt-get install python-pip
sudo pip install catkin_pkg
```

#### Hexagon SDK 3.0

Clone the following:
```
git clone https://github.com/ATLFlight/cross_toolchain
```

Download the [Hexagon SDK 3.0 for Linux](https://developer.qualcomm.com/download/hexagon/hexagon-sdk-v3-linux.bin). You will have to use a browser as it requires QDN registration and a click through.

Copy the files to the download dir:
```
cp ~/Downloads/{name of file downloaded, eg. qualcomm_hexagon_sdk_lnx_3_0_eval.bin} cross_toolchain/downloads
```

Now run the install script:
```
cd cross_toolchain
./installv3.sh
```

The script will prompt you to optionally update the default installation path ${HEXAGON_INSTALL_HOME} and then set the following environment variables after the installation:
```
export HEXAGON_SDK_ROOT=${HEXAGON_INSTALL_HOME}/Qualcomm/Hexagon_SDK/3.0
export HEXAGON_TOOLS_ROOT=${HEXAGON_INSTALL_HOME}/Qualcomm/HEXAGON_Tools/7.2.12/Tools
export PATH=${HEXAGON_SDK_ROOT}/gcc-linaro-4.9-2014.11-x86_64_arm-linux-gnueabihf_linux/bin:$PATH
```

To prevent the path from having multiple versions of the ARM cross compiler path you can do:

```
[ `echo PATH | grep gcc-linaro-4.9-2014.11-x86_64_arm-linux-gnueabihf_linux/bin`' ] || \
 	export PATH=${HEXAGON_SDK_ROOT}/gcc-linaro-4.9-2014.11-x86_64_arm-linux-gnueabihf_linux/bin:$PATH
```

### Set up the Sysroot

A sysroot is required to provide the libraries and header files needed to cross compile applications for
the Snapdragon Flight applications processor.

__NOTE__: The qrlSDK sysroot is currently unavailable from Intrinsyc so please use the stock Ubuntu 14.04 sysroot (Trusty).

#### Create the stock Ubuntu Trusty (14.04) sysroot

```
export HEXAGON_SDK_ROOT=${HEXAGON_INSTALL_HOME}/Qualcomm/Hexagon_SDK/3.0
./trusty_sysroot.sh
```
This will install:

ARMv7hf Ubuntu Trusty (14.04) sysroot [HEXAGON_ARM_SYSROOT]: ${HOME}/Qualcomm/ubuntu_14.04_armv7_sysroot

Make sure to set the environment variable:
```
export HEXAGON_ARM_SYSROOT=${HEXAGON_INSTALL_HOME}/Qualcomm/ubuntu_14.04_armv7_sysroot
```
## All Done. What Next?

You have installed all the prerequisites to build code for the Hexagon DSP. Try the [HelloWorld](HelloWorld.md)
program to test running a program on the DSP.
