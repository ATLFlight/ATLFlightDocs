# Getting Started

These instructions are for using DSPAL and DriverFramework with the Hexagon Toolchain on x86_64 Linux.

## Prerequisites

### Linux Host Setup

You will need clang 3.4.2 or greater or GCC 4.8 or greater, and cmake 2.8 or greater.

These instuctions assume that you follow the default installation proceedure for the Hexagon SDK and Tools.
The packages will be installed to ~/Qualcomm/...

The top working dir is assumed to be the user home directory (~), and downloads are assumed to be in
~/Downloads for simplicity.

#### CMake

You can get the latest version of cmake from the website:

```
cd ~/Downloads
wget https://cmake.org/files/v3.4/cmake-3.4.0-Linux-x86_64.sh
sudo mkdir /opt/cmake-3.4.0
sudo sh ./cmake-3.4.0-Linux-x86_64.sh --prefix=/opt/cmake-3.4.0 --exclude-subdir
export PATH=/opt/cmake-3.4.0/bin:${PATH}
```

Add the following to your .bashrc or equivalent for your preferred shell so the path is
updated automatically.

```
if [ "x`echo $PATH | grep '/opt/cmake-3.4.0/bin'`" = "x" ]; then
  PATH=$PATH:/opt/cmake-3.4.0/bin
fi
export PATH=$PATH
```

#### Ubuntu 12.04
For Ubuntu 12.04 you can use clang 3.4.2 from the LLVM downloads page:

```
wget http://llvm.org/releases/3.4.2/clang+llvm-3.4.2-x86_64-unknown-ubuntu12.04.xz
```
#### 

#### Ubuntu 14.04 to Ubuntu 15.10
Install the system version of clang and/or GCC:

```
sudo apt-get install gcc g++ clang clang++
```

### Hexagon SDK and Hexagon Tools for Linux

Request the access to the Hexagon SDK and Hexagon Tools from https://developer.qualcomm.com/software/hexagon-dsp-sdk/application.
Specifically, request access to the following files for use with PX4:

```
qualcomm_hexagon_sdk_2_0_eval.bin
Hexagon.LLVM_linux_installer_7.2.10.bin 
```

Install the Hexagon SDK to $HOME/Qualcomm/Hexagon_SDK. This is the default installation path.
You can unselect the install of Eclipse if you don't plan to use Eclipse. This will install version 6.4.06 if the Hexagon Tools. 
A newer version of Hexagon Tools will be installed next.

```
sudo sh ./qualcomm_hexagon_sdk_2_0_eval.bin
```

Install the Hexagon Tools to $HOME/Qualcomm/HEXAGON_Tools. This is the default installation path.

```
sudo sh ./Hexagon.LLVM_linux_installer_7.2.10.bin
```

Set the following environment variable to the install path:

```
export HEXAGON_SDK_ROOT=${HOME}/Qualcomm/Hexagon_SDK/2.0
export HEXAGON_TOOLS_ROOT=${HOME}/Qualcomm/HEXAGON_Tools/7.2.10/Tools
```

# Setup qaic IDL compiler, and rpcmem.h

After installing the SDK, You need to set up the qaic IDL compiler and rpcmem.h which can be done by building one of the example applications..

```
cd ${HEXAGON_SDK_ROOT}
source setup_sdk_env.sh
cd examples/common/calculator
make tree V=UbuntuARM_Debug
make tree V=UbuntuARM_Release
make tree V=hexagon_Release_dynamic_v5
make tree V=hexagon_Debug_dynamic_v5
```

This will create ${HEXAGON_SDK_ROOT}/tools/Linux/qaic compatible with your Linux version. If you omit this step,
build rules that call qaic will fail.

It will also create required header files in the appropriate "ship" directories.

# Setup the cross build tools for building Krait/ARM apps

The following will install the ARM cross compiler and the sysroot for building applications.

```
cd ~
git clone https://github.com/ATLFlight/cross_toolchain
cd cross_toolchain
./install.sh
```

## All Done. What Next?

You have installed all the prerequisites to build code for the Hexagon DSP. Try the [HelloWorld](HelloWorld.md)
program to test running a program on the DSP.

