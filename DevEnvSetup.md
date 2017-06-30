# Snapdragon Flight Development Environment Setup

## SDK and Tools
The following SDK and tools are available to develop applications on the Snapdragon Flight™ platform.
  * The qrlSDK is used used to create applications that run on the ARM-based applications processor. Once installed, the SDK provides:
    * The gcc cross-toolchain that is downloaded from the Linaro servers
    * The include files and libraries required to compile and link userspace applications that run on the apps processor
    * The environment setup script environment-setup-<target> required by the cross-toolchain
  * The Hexagon™ SDK and Hexagon™ tools are used to create applications that run on the Hexagon™ DSP.

## Prerequisites
A x86_64 Ubuntu (14.04 or higher) workstation is required

## Setup
The steps for installation of both SDK and tools are scripted and documented at https://github.com/ATLFlight/cross_toolchain. Follow the instructions in [the README](https://github.com/ATLFlight/cross_toolchain#cross_toolchain).

### Environment Variables Changes
At the end of the installation, the script will display instructions to set your environment variables.

Below is an example. Actual values to be used based on the instructions displayed by the installation script.

```
export ${HEXAGON_INSTALL_HOME}=${HOME}
export HEXAGON_SDK_ROOT=${HEXAGON_INSTALL_HOME}/Qualcomm/Hexagon_SDK/3.0
export HEXAGON_TOOLS_ROOT=${HEXAGON_INSTALL_HOME}/Qualcomm/HEXAGON_Tools/7.2.12/Tools
export HEXAGON_ARM_SYSROOT=${HEXAGON_INSTALL_HOME}/Qualcomm/qrlinux_v4_sysroot/merged-rootfs
export ARM_CROSS_GCC_ROOT=${HEXAGON_INSTALL_HOME}/Qualcomm/ARM_Tools/gcc-4.9-2014.11
```

