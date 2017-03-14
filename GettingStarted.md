# Getting Started

These instructions are for using DSPAL and DriverFramework with the Hexagon Toolchain on x86_64 Linux.

## Prerequisites

### Linux Host Setup

These instuctions describe the default installation proceedure for the Hexagon SDK and Tools.
The packages will be installed to ~/Qualcomm/...

The top working dir is assumed to be the user home directory (~), and downloads are assumed to be in
~/Downloads for simplicity.

#### 1. Install clang 3.4.2 or greater, and GCC 4.9 or greater, and CMake 3.4.3 or greater

Do not use cmake 3.4.0. It is known to have issues that break the PX4 build. Later versions (3.4.3+) are fine.

##### Ubuntu 14.04

The version of cmake in Ubuntu 14.04 is too old, so a newer version must be downloaded.

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

Install the compilers

```
sudo apt-get install clang clang++ gcc g++ 
```

##### Ubuntu 16.04

Install the compilers and cmake:

```
sudo apt-get install clang clang++ gcc g++ cmake 
```

#### 3. Install other packages

```
sudo apt-get install python-empy
sudo apt-get install python-pip
sudo pip install catkin_pkg
```

#### Hexagon SDK 3.0 and qrlSDK

Clone the following:
```
git clone https://github.com/ATLFlight/cross_toolchain
```

Download the [Hexagon SDK 3.0 for Linux](https://developer.qualcomm.com/download/hexagon/hexagon-sdk-v3-linux.bin). You will have to use a browser as it requires QDN registration and a click through.

Download [qrlsdk](http://support.intrinsyc.com/attachments/download/1011/qrlSDK.tgz). You will need a Intrinsyc login to access the file.

Copy the files to the download dir:
```
cp ~/Downloads/{name of file downloaded, eg. qualcomm_hexagon_sdk_lnx_3_0_eval.bin} cross_toolchain/downloads
```

Now run the install script:
```
cd cross_toolchain
./installsdk.sh --APQ8074 --arm-gcc --qrlSDK
```

To install a stripped down version of the SDK use (for use in Docker image):
```
./installsdk.sh --APQ8074 --trim --arm-gcc --qrlSDK
```

##### Setup Environment Variables
The script will prompt you to optionally update the default installation path ${HEXAGON_INSTALL_HOME} and uses the following environment variables for the installation.
Assuming you select the default install path of ${HOME} the environment settings would be:
```
export ${HEXAGON_INSTALL_HOME}=${HOME}
export HEXAGON_SDK_ROOT=${HEXAGON_INSTALL_HOME}/Qualcomm/Hexagon_SDK/3.0
export HEXAGON_TOOLS_ROOT=${HEXAGON_INSTALL_HOME}/Qualcomm/HEXAGON_Tools/7.2.12/Tools
export PATH=${HEXAGON_SDK_ROOT}/gcc-linaro-4.9-2014.11-x86_64_arm-linux-gnueabihf_linux/bin:$PATH
export HEXAGON_ARM_SYSROOT=${HEXAGON_INSTALL_HOME}/Qualcomm/qrlinux_v4_sysroot/merged-rootfs
```

To prevent ${PATH} from having multiple versions of the ARM cross compiler path you can do:

```
[ `echo PATH | grep gcc-linaro-4.9-2014.11-x86_64_arm-linux-gnueabihf_linux/bin`' ] || \
 	export PATH=${HEXAGON_SDK_ROOT}/gcc-linaro-4.9-2014.11-x86_64_arm-linux-gnueabihf_linux/bin:$PATH
```

Make sure these variables are set when building code using the Hexagon SDK and Hexagon Tools.

## All Done. What Next?

You have installed all the prerequisites to build code for the Hexagon DSP. Try the [HelloWorld](HelloWorld.md)
program to test running a program on the DSP.
