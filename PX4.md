# PX4

This document describes the build and execution procedure for PX4 flight software on the [Snapdragon Flight](https://www.intrinsyc.com/vertical-development-platforms/qualcomm-snapdragon-flight) platform using drivers provided by Qualcomm.

## Prerequisites

To build the PX4 flight stack for Snapdragon Flight, you need to set up the build environment as described in [GettingStarted](GettingStarted.md).
You can optionally build and run [HelloWorld](https://github.com/ATLFlight/ATLFlightDocs/blob/master/HelloWorld.md) to test your setup.

## Building PX4 with the upstream drivers

The official PX4 documentation is at [http://dev.px4.io/](http://dev.px4.io/).

The build should be as simple as:
```
git clone https://github.com/PX4/Firmware
cd Firmware
git submodule update --init --recursive
make eagle_default
```
Load the code on the device
```
adb push ./build_posix_eagle_default/src/firmware/posix/mainapp /home/linaro
adb push ./build_qurt_eagle_default/src/firmware/qurt/libmainapp.so /usr/share/data/adsp
adb push ./build_qurt_eagle_default/src/firmware/qurt/libpx4muorb_skel.so /usr/share/data/adsp
```

## Building PX4 with the FC_ADDON drivers

The FC_ADDON contains generic proprietary drivers for the rc_receiver and uart_esc. The upstream PX4 project contains wrappers for these drivers for use by PX4.

Login to the Intrinsyc support website and download the latest Flight Controller AddOn. The Flight Controller AddOn for Snapdragon Flight provides some driver binaries that require wrappers for use with PX4. This repository contains these driver wrappers.

Following these instructions to build the PX4 flight code for Snapdragon Flight using the driver binaries provided by Qualcomm.

### Host Setup

Follow the instructions [here](https://github.com/ATLFlight/ATLFlightDocs) to install the tools, setup the environment variables.

Obtain the Flight Controller AddOn file (qcom_flight_controller_*.zip). The latest version is available from the [Intrinsyc support website](http://support.intrinsyc.com/projects/snapdragon-flight/files). Download and extract it to any location on your linux PC. The environment variable FC_ADDON needs to point to the location of the Snapdragon Flight addon.

To use these with PX4, do the following:

```
git clone https://github.com/ATLFlight/Firmware
cd Firmware
git checkout tags/<stable_release_tag_name>
git submodule update --init --recursive
export FC_ADDON=<location-of-extracted-flight-controller-addon>
```
*NOTE:* Substitute \<stable_release_tag_name\> with the latest tag from the *Stable Releases* section on this page.

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
- hil: Configuration files for hardware-in-the-loop (HIL) simulation
- flight: Configuration files for a generic flight platform
- 200qx: Configuration files customized for the Microheli 200 MM flight platform

Install the configuration files:
```
cd Firmware
adb push ./posix-configs/eagle/flight/px4.config /usr/share/data/adsp/px4.config
adb push ./posix-configs/eagle/flight/mainapp.config /home/linaro/mainapp.config
```

*NOTE:* The steps above installed the flight config files for a generic flight platform. Please modify the source path and file names based on your platform or use-case.

Install the PX4 binaries:
```
cd Firmware
adb push ./build_posix_eagle_legacy_driver_default/src/firmware/posix/mainapp /home/linaro
adb push ./build_qurt_eagle_legacy_driver_default/src/firmware/qurt/libmainapp.so /usr/share/data/adsp
adb push ./build_qurt_eagle_legacy_driver_default/src/firmware/qurt/libpx4muorb_skel.so /usr/share/data/adsp
```

Install the aDSP static image and drivers from the Flight Controller AddOn:
```
adb push ${FC_ADDON}/flight_controller/hexagon/libs/. /usr/share/data/adsp
adb push ${FC_ADDON}/images/8074-eagle/normal/adsp_proc/obj/qdsp6v5_ReleaseG/LA/system/etc/firmware/. /lib/firmware
```

## Execution

Connect to the target via SSH (recommended) or ADB. Then start the PX4 flight software as follows:
```
sudo su
cd /home/linaro
chmod a+x mainapp
./mainapp mainapp.config
```

To stop the flight stack and exit gracefully, run the following commands in the mainapp shell:
```
muorb stop
shutdown
```

## Stable Releases
Following are the git tags corresponding to stable releases:
- EAGLE_DRONE_1.1_PCS_0 (June 6, 2016)
- EAGLE_DRONE_1.1_PCS_1 (June 28, 2016)
- EAGLE_DRONE_1.2_ES2_0 (July 12, 2016)

## Known Issues and Limitations

*NOTE:* This section is applicable to the latest stable release only.

Only the following modes / configurations / tools have been tested and therefore supported:
- Flight modes
  - MANUAL (Angle) mode
  - ATLCTL mode
- Quadcopter X airframes
- QGroundControl 2.7.1 (available [here](https://github.com/mavlink/qgroundcontrol/releases/tag/v2.7.1))

The following are not functional:
- HITL / HIL mode
- Accelerometer calibration through the QGroundControl user interface

## More Information
- Running the mini-dm logging tool: http://dev.px4.io/starting-building.html#run-it
- Setting up auto-start of mainapp upon boot up: http://dev.px4.io/starting-building.html#autostart-mainapp
- Troubleshooting: http://dev.px4.io/advanced-snapdragon.html
- PX4 User Guide: http://px4.io/user-guide
- PX4 Developer Guide: http://dev.px4.io
