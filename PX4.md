# PX4

This document describes the build and execution procedure for PX4 flight software on the [Snapdragon Flight](https://www.intrinsyc.com/vertical-development-platforms/qualcomm-snapdragon-flight) platform using drivers provided by Qualcomm.

1. [Prerequisites](#prerequisites)
1. [Building with upstream drivers](#building-with-upstream-drivers)
1. [Building with FC_ADDON drivers](#building-with-fc_addon-drivers)
  1. [Host Setup](#host-setup)
  1. [Building the Code](#building-the-code)
  1. [Installation on Target](#installation-on-target)
  1. [Execution](#execution)
1. [Known Issues and Limitations](#known-issues-and-limitations)
1. [More Information](#more-information)

## Prerequisites

To build the PX4 flight stack for Snapdragon Flight, you need to set up the build environment as described in [GettingStarted](GettingStarted.md).
You can optionally build and run [HelloWorld](https://github.com/ATLFlight/ATLFlightDocs/blob/master/HelloWorld.md) to test your setup.

## Building with upstream drivers

This configuration is supported by the PX4 community. For more information on the hardware components required, please see [http://dev.px4.io](http://dev.px4.io/).

To build code for this configuration, please SKIP the rest of this page and follow the instructions in the official PX4 documentation at [http://dev.px4.io/starting-building.html](http://dev.px4.io/starting-building.html).

## Building with FC_ADDON drivers

The following hardware is supported / tested by this configuration:
* [Snapdragon Flight](https://shop.intrinsyc.com/products/qualcomm-snapdragon-flight-sbc) or [Developer's Edition](https://shop.intrinsyc.com/products/snapdragon-flight-dev-kit)
* [Qualcomm ESC board](http://shop.intrinsyc.com/products/qualcomm-electronic-speed-control-board)
* Spektrum DSM RC Receiver (such as [AT6210](http://www.spektrumrc.com/Products/Default.aspx?ProdID=SPMAR6210)), and a [Compatible Radio](https://www.spektrumrc.com/Air/Radios.aspx)
* MicroHeli 200mm quadcopter airframe (other airframes may work too)

This configuration uses proprietary UART_ESC and RC Receiver drivers that are provided by the Flight Controller AddOn (FC_ADDON). The upstream PX4 project contains wrappers for these drivers for use by PX4.

Login to the [Intrinsyc support website](http://support.intrinsyc.com/projects/snapdragon-flight/files) and download the latest Flight Controller AddOn. The Flight Controller AddOn for Snapdragon Flight provides some driver binaries that require wrappers for use with PX4. This repository contains these driver wrappers.

Following these instructions to build the PX4 flight code for Snapdragon Flight using the driver binaries provided by Qualcomm.

### Host Setup

Follow the instructions [here](https://github.com/ATLFlight/ATLFlightDocs/blob/master/DevEnvSetup.md) to install the tools, setup the environment variables.

Obtain the Flight Controller AddOn file (\*\_qcom_flight_controller\_\*.zip). The latest version is available from the [Intrinsyc support website](http://support.intrinsyc.com/projects/snapdragon-flight/files). Download and extract it to any location on your linux PC. The environment variable FC_ADDON needs to point to the location of the extracted Snapdragon Flight Controller addon.

To use these with PX4, do the following:

```
git clone https://github.com/PX4/Firmware
cd Firmware
git checkout tags/v1.7.3
git submodule update --init --recursive
export FC_ADDON=<location-of-extracted-flight-controller-addon>
```

*NOTE:*
- The above git tag is compatible/was tested with platform software version 3.1.3.1 and flight controller addon version 3.1.3.1 available from Intrinsyc [here](http://support.intrinsyc.com/projects/snapdragon-flight/files). Follow the [instructions](https://support.intrinsyc.com/projects/snapdragon-flight/wiki/Get_and_install_the_latest_platform_BSP) therein to complete the installation.
- Later releases at https://github.com/PX4/Firmware/releases MAY work too but have not been tested.

### Building the Code
The commands below build the targets for the DSP and the Linux side. Both executables communicate via muORB.
```
cd Firmware
make eagle_legacy_default
```

### Installation on Target
To load the software on the device, make sure the device is booted up, and verify that you can connect to it via USB cable or SSH.

The PX4 software uses configuration files that need to be copied on target. They are located in the following path:
```
Firmware/posix-configs/eagle/<sub-directory>
```

Where <sub-directory> refers to the platform or use-case such as:
* hil: Configuration files for hardware-in-the-loop (HIL) simulation
* flight: Configuration files for a generic flight platform
* 200qx: Configuration files customized for the Microheli 200 MM flight platform

Install the configuration files:
```
cd Firmware
adb push ./posix-configs/eagle/200qx/px4.config /usr/share/data/adsp/px4.config
adb push ./posix-configs/eagle/200qx/mainapp.config /home/linaro/mainapp.config
```

*NOTE:* The steps above installed the flight config files for the 200mm flight platform. Please modify the source path and file names based on your platform or use-case.

Install the PX4 binaries:
```
cd Firmware
adb push ./build/posix_eagle_legacy/px4 /home/linaro
adb push ./build/qurt_eagle_legacy/platforms/qurt/libpx4.so /usr/share/data/adsp
adb push ./build/qurt_eagle_legacy/platforms/qurt/libpx4muorb_skel.so /usr/share/data/adsp
```

If not already done, install the aDSP static image and drivers from the Flight Controller AddOn by executing the install script provided in the addon:
```
installfcaddon.sh <from Linux>
<or>
installfcaddon.bat <from Windows>
```

## Execution  
Connect to the target via SSH (recommended) or ADB. 

Optionally, remove the following files (that may have been created from a previous run)
```
sudo su
cd /home/linaro
rm -rf log
rm -rf dataman
rm -rf rootfs 
```

Start the PX4 flight software as follows:
```
sudo su
cd /home/linaro
chmod a+x px4
./px4 mainapp.config
```

To stop the flight stack and exit gracefully, run the following commands in the mainapp shell:
```
muorb stop
shutdown
```

## Known Issues and Limitations

*NOTE:* This section is applicable to the latest [stable release](#stable-releases) only.

Only the following modes / configurations / tools have been tested and therefore supported:
- Flight modes
  - MANUAL (Angle) mode
  - ATLCTL mode
- Quadcopter X airframes

The following are not functional:
- HITL / HIL mode

## More Information
- Running the DSP debug monitor (mini-dm logging tool): https://dev.px4.io/en/debug/system_console.html
- Setting up auto-start of mainapp upon boot up: https://dev.px4.io/en/setup/building_px4.html#qurt--snapdragon-based-boards
- Troubleshooting: https://docs.px4.io/en/flight_controller/snapdragon_flight_advanced.html
- PX4 User Guide: https://docs.px4.io/en
- PX4 Developer Guide: https://dev.px4.io/en
