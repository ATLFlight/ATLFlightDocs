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
#### Hexagon SDK 3.0

Clone the following:
```
git clone https://github.com/ATLFlight/cross_toolchain
cd cross_toolchain
```

Download the [Hexagon SDK 3.0 for Linux](https://developer.qualcomm.com/download/hexagon/hexagon-sdk-v3-linux.bin). You will have to use a browser as it requires QDN registration and a click through.

Copy the files to the download dir:
```
cp ~/Downloads/hexagon-sdk-v3-linux.bin cross_toolchain/downloads
```

Now run the install script:
```
cd cross_toolchain
./installv3.sh
```

The script will prompt you to set the following environment variables:
```
export HEXAGON_SDK_ROOT=${HOME}/Qualcomm/Hexagon_SDK/3.0
export HEXAGON_TOOLS_ROOT=${HOME}/Qualcomm/HEXAGON_Tools/7.2.12/Tools
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

Login to the Intrinsyc support page and download: http://support.intrinsyc.com/attachments/download/483/Flight_qrlSDK.zip

copy/move the file to the ./download directory

```
cp ~/Downloads/Flight_qrlSDK.zip ./downloads
./qrlinux_sysroot.sh --clean
```

Set the following environment variable:

```
export HEXAGON_ARM_SYSROOT=${HOME}/Qualcomm/qrlinux_v1.0_sysroot
```

For more sysroot options see [Sysroot Installation](https://github.com/ATLFlight/cross_toolchain/blob/sdk3/README.md#sysroot-installation)

## All Done. What Next?

You have installed all the prerequisites to build code for the Hexagon DSP. Try the [HelloWorld](HelloWorld.md)
program to test running a program on the DSP.
