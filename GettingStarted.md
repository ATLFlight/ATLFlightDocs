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

If you do not wish to use the Hexagon Tools 7.2.10 version (or if you don't have it), you can set the HEXAGON_TOOLS_ROOT to the 6.4.06 tools installed by the SDK installer. In this case you will want to leave the Hexagon Tools option enabled during the SDK install.

```
export HEXAGON_TOOLS_ROOT=${HOME}/Qualcomm/HEXAGON_Tools/6.4.06
```

Otherwise, to use the Hexagon Tools 7.2.10 you can leave HEXAGON_TOOLS_ROOT unset or set it to:
```
export HEXAGON_TOOLS_ROOT=${HOME}/Qualcomm/HEXAGON_Tools/7.2.10/Tools
```
Now run the install script:
```
cd cross_toolchain
./install.sh
```

You will need to set the following environment variables:
```
export HEXAGON_SDK_ROOT=${HOME}/Qualcomm/Hexagon_SDK/2.0
export HEXAGON_TOOLS_ROOT=${HOME}/Qualcomm/HEXAGON_Tools/7.2.10/Tools
export HEXAGON_ARM_SYSROOT=${HOME}/Qualcomm/Hexagon_SDK/2.0/sysroot
export PATH=${HEXAGON_SDK_ROOT}/gcc-linaro-arm-linux-gnueabihf-4.8-2013.08_linux/bin:$PATH
```

## All Done. What Next?

You have installed all the prerequisites to build code for the Hexagon DSP. Try the [HelloWorld](HelloWorld.md)
program to test running a program on the DSP.

