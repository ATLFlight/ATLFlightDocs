# Getting Started with apps processor (CPU) development

This page provides information on setting up your environment to build and run code on the applications processor (CPU).

1. [Prerequisites](#prerequisites)
1. [Building and running sample app](#building-and-running-sample-app)
1. [More information on environment setup](#more-information-on-environment-setup)
   * [Shell limitation](#shell-limitation)
1. [SDK capabilities and limitations](#sdk-capabilities-and-limitations)

## Prerequisites
Install the qrlSDK using the instructions at [Development Environment Setup](DevEnvSetup.md).

## Building and running sample app
A "Hello World" sample app is included in the SDK tree.

To build the sample app:
```
> cd /local/mnt/workspace/qrlSdkSrc/sample
> source /local/mnt/workspace/qrlSdkDst/environment-setup-<target>
> make
```

*WARNING:* See [Shell Limitations](#shell-limitation)

Push the sample app to the target and run it:
```
> adb push hello /home/linaro
<xyz> KB/s (<abc> bytes in <p.q>s)
> adb shell /home/linaro/hello
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
