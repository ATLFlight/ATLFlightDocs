# Getting Started with apps processor (CPU) development

This page provides information on setting up your environment to build and run code on the applications processor (CPU).

1. [Introduction to SDK](#introduction-to-sdk)
1. [SDK Installation](#sdk-installation)
1. [Building and running sample app](#building-and-running-sample-app)
1. [More information on environment setup](#more-information-on-environment-setup)
  * [Shell limitation](#shell-limitation)
1. [SDK capabilities and limitations](#sdk-capabilities-and-limitations)

## Introduction to SDK
The software development kit (SDK) is a cross-development toolkit for the advanced RISC machines (ARM) apps processor in the Snapdragon™ system on chip (SOC). The SDK allows developers to compile and link applications meant for the ARM CPU on their x86-64 based host computer, and then to deploy them on-target.

Once installed, the SDK provides:
- The gcc cross-toolchain, which is downloaded from the Linaro servers
- The include files and libraries required to compile and link userspace applications that run on the apps processor
- The environment setup script environment-setup-<target> required by the cross-toolchain

## SDK Installation
Complete the installation of the qrlSDK package that is part of the Snapdragon Flight™ platform.

1. Download the qrlSDK ZIP file from [here](http://support.intrinsyc.com/projects/snapdragon-flight/files).
2. Extract the file in the desired location on the host computer.
3. Run the installer from the extracted files. Provide it the SDK source and the destination to install it to.

Example:
- SDK archive was extracted at ```/local/mnt/workspace/qrlSdkSrc```
- SDK to be installed at ```/local/mnt/workspace/qrlSdkDst```

```
# Create the source and destination directories
> mkdir -p /local/mnt/workspace/qrlSdkSrc
> mkdir -p /local/mnt/workspace/qrlSdkDst

# Extract the SDK installer archive
> cd /local/mnt/workspace/
> unzip <path>/Flight_<version>_qrlSDK.zip
> cd qrlSDK

# Install the SDK
> ./qrlSDKInstaller.sh -s . -d /local/mnt/workspace/qrlSdkDst

# IF you want to view the usage help
> ./qrlSDKInstaller.sh -h
```

## Building and running sample app
A "Hello World" sample app is included in the SDK archive.

To build the sample app:
```
> cd /local/mnt/workspace/qrlSdkSrc/qrlSDK/sample
> source /local/mnt/workspace/qrlSdkDst/environment-setup-<target>
> make
```

*WARNING:* See [Shell Limitations](#shell-limitation)

Push the sample app to the target and run it:
```
> adb push hello /home/linaro
<xyz> KB/s (<abc> bytes in <p.q>s)
> adb shell /home/linaro
Hello world!
```

## More information on environment setup
The environment setup sets environment variables like CC, CFLAGS, LDFLAGS, so you can source this file, then use a Makefile for the build. The environment variables provide the variables $CC, $CFLAGS, etc., to the Makefile, making it simple to compile/link your code. The environment setup also includes passing --sysroot=xxx to the gcc so the include file and libraries used on the target can be linked with your code.

### Shell limitation
Sourcing the SDK's environment setup script renders the user's shell unusable for normal operations other than cross-compilation for the Snapdragon Flight™ apps processor applications. For instance, the ```adb``` may not work. It is suggested that users open another shell to do other operations.

## SDK capabilities and limitations
The following types of applications were tested and are supported by this SDK:
- [Camera apps](https://github.com/ATLFlight/ATLFlightDocs/blob/master/CameraProg.md)

The SDK does NOT support building of the following software. They need to be built on target.
- [VISLAM ROS example](https://github.com/ATLFlight/ros-examples)
