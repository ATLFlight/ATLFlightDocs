# Snapdragon Flight User Guide
This page provides useful information about the Qualcomm® Snapdragon™ Flight Kit (Developer's edition) available [here](https://www.intrinsyc.com/vertical-development-platforms/qualcomm-snapdragon-flight). The Qualcomm® Snapdragon Flight™ is a highly integrated board based on a Qualcomm® Snapdragon™ 801 series processor that targets consumer drones and robotics applications.

# Table of Contents
1. [Logging](#logging)
   - [aDSP](#adsp)
   - [Apps processor](apps-processor)
1. [Flight Control](#flight-control)
   1. [WiFi control](#wifi-control)
   1. [RC radio control](#rc-radio-control)
1. [aDSP SDK](#adsp-sdk)
1. [Device path to hardware port mapping and runtime configuration](#device-path-to-hardware-port-mapping-and-runtime-configuration)
1. [Camera](#camera)
1. [Acronyms and abbreviations](#acronyms-and-abbreviations)
1. [Additional references](#additional-references)

## Logging
### aDSP
The mini-diagnostic monitor (mini-dm) is used to get debug messages from the DSP (including the FastRPC application) using the following procedure:

1. Download and install  the [Hexagon SDK](https://developer.qualcomm.com/software/hexagon-dsp-sdk/tools) on your host machine.
2. Navigate to the mini-dm executable location: ``` ${HEXAGON_INSTALL_HOME}/Qualcomm/Hexagon_SDK/3.0/tools/debug/mini-dm/Linux_Debug ```
3. Connect the USB cable from the Snapdragon Flight board to the host machine where the mini-dm is installed.
4. Run the mini-dm using the following command: ``` ./mini-dm ```
5. Run the FastRPC or other application that loads the aDSP module. For more information, see the Hexagon SDK documentation located at ```${HEXAGON_INSTALL_HOME}/Qualcomm/Hexagon_SDK/3.0/docs/index.html```
6. For more information on mini-dm, see this [FAQ](https://developer.qualcomm.com/forum/qdn-forums/mobile-technologies/multimedia-optimization-hexagon-sdk/toolsinstallation/27111).
### Apps processor
#### Serial cable
  TODO: Information coming soon.

## Flight Control
Flight control commands are transmitted over a Wi-Fi or RC communication link.
### WiFi control
The Wi-Fi communication link can transmit commands to the unmanned aerial vehicle (UAV). 
For information on installation of antennas for Wi-Fi, see Qualcomm Snapdragon Flight User Guide (80-H9581-1).
#### UAV setup
By default, the system should come up in SoftAP mode. If the default is not set, this configuration can be enable with the following steps:  
   1. Enable AP mode and reboot. See [here](http://dev.px4.io/advanced-snapdragon.html#wifi-settings) for additional information.  
   2. The AP mode defaults with automatic channel selection (ACS), which selects a 2.4 GHz channel with the least interference. If this is not the desired provisioning, this can be configured manually. Scan for other APs in the vicinity and configure SoftAP with a channel unlikely to suffer from interference with other devices. See the official [User Guide](http://support.intrinsyc.com/documents/125) for additional information.  
   3. Once SoftAP mode is enabled, run the following command (through adb or serial console to the UAV) to determine its server set identifier (SSID):
   ```/usr/local/qr-linux/wificonfig.sh –g```  
   4. When the PX4 flight stack is installed, this should include provisioning for MavLink communication to allow commands to be received and used by the flight stack. See the [QGroundControl documentation](https://donlakeflyer.gitbooks.io/qgroundcontrol-user-guide/content) for additional details.
#### View FPV video stream
It also is possible to stream the FPV from the drone. This feature currently is currently in alpha state.
Connect to the drone and start the flight software as usual. The FPV reads from an RTSP server running on the drone.
The RTSP server can be started manually using the following command:  
```qcamvid –c hires –f auto –o /dev/null –t 600```    
The argument to “-t” is the video stream duration in seconds (the video stream will stop after that length of time, which will cause the FPV on the app to fail since there is no more video streaming).  
* -c is the type of camera.
* -f is the focus mode. The example command above sets it in Auto mode.
* -o is video output file name. The example command above does not keep the video file, and instead pipes it to /dev/null.

*NOTE:*	For more information on these options, use the qcamvid --help option.  

The MPlayer application can be used to stream the video to a Linux and Windows laptop. For instance, users can run MPlayer on a Windows laptop while connected to the drone in AP mode.  
* Command to start mplayer on a laptop: ``` mplayer rtsp://<ip address of the drone>/fpvview --nocache --fps 30```
* If the drone is in AP mode, the command is:
```mplayer rtsp://192.168.1.1/ fpvview --nocache --fps 30```
* If the drone is in station mode, where the IP address of the drone is 10.73.189.56, the command is:
```mplayer rtsp://10.73.189.56 fpvview --nocache --fps 30```

*NOTE:* If there are any issues with the streaming, restart the client and server apps.
#### Spektrum transmission over WiFi
The Spektrum transmitter can send control commands over Wi-Fi.  
When running the PX4 flight stack for instance, a standard RC transmitter can be connected via the AeroSIM RC dongle to a laptop computer running the QGroundControl application. The RC transmitter relays commands to the QGroundControl application, which are then transmitted to the UAV over Wi-Fi.
#### Setup QGroundControl application
When running the PX4 flight stack, the QGroundControl application running on a Wi-Fi-enabled Windows laptop computer needs to be set up as per the instructions here: https://donlakeflyer.gitbooks.io/qgroundcontrol-user-guide/content  
### RC radio control
Connect your RC receiver to the Snapdragon Flight board.  
To set up your transmitter, see the instruction manual at http://www.spektrumrc.com/Air/Radios.aspx corresponding to your make and model (tested with DX6i or later).  
To bind your transmitter to the receiver on your UAV, see the binding section of the manual corresponding to your receiver: http://www.spektrumrc.com/Air/Receivers.aspx.  
Once the system has powered up, the transmitter joysticks and switches may be used to control and operate the UAV (including arm, disarm, pitch, yaw, roll, altitude, and navigation).  
Consult your flight stack software documentation for details on operating the UAV using the transmitter in various modes of operation. (For PX4, see https://pixhawk.org/peripherals/radio-control/start and http://px4.io/docs/px4-autopilot/flying). 

## aDSP SDK  
The SDK includes the following:
  - The Flight Controller AddOn file (*_qcom_flight_controller_*.zip) which is the Qualcomm add-on for the flight controller. The latest version is available from the [Intrinsyc support website](http://support.intrinsyc.com/projects/snapdragon-flight/files).
    - NOTE: A default version of the add-on is pre-installed on target. In some cases, a new add-on may be available at the above URL that may contain additional features or fixes. In that situation, please download and extract the ZIP file, and execute the installation script therein to install the contents on target.
  - The Hexagon SDK which provides a complete environment and means for generating dynamic Hexagon DSP code to customize and extend the features of the aDSP. Refer to the following link for the general Hexagon Digital Signal Process (DSP) SDK development guide: https://developer.qualcomm.com/software/hexagon-dsp-sdk.

### aDSP/Hexagon developer guide
Refer to the file docs/index.html in the Hexagon SDK available from https://developer.qualcomm.com/software/hexagon-dsp-sdk/tools.  

### DSP abstraction layer API
The DSP abstraction layer (DSPAL) provides a standard interface for porting code to the application digital signal processor (aDSP) (Hexagon) processor. The DSPAL code and more information is available [here](DSPAL.md) and [here](https://github.com/ATLFlight/dspal).

## Device path to hardware port mapping and runtime configuration
### Overview
The APQ8074 has 12 BLSP ports, each of which can be configured as serial peripheral interface (SPI), inter-integrated circuit (I2C), or universal asynchronous receiver transmitter (UART), etc. These ports are shared between the aDSP and the apps processor. To isolate the BAM low-speed peripheral (BLSP) port usage between the two subsystems, we can define BLSP port allocation in the TrustZone (TZ) module. Subsystems that access BLSP ports not owned by them result in a system crash.
In DspAL, we provide a device path to hardware mapping in the following manner:
   - SPI: ```/dev/spi-[1-12]``` represents SPI device on BLSP 1 to 12
   - I2C:
      - ```/dev/i2c-[0-11]``` represents I2C device on BLSP 1 to 12  
      OR
      - ```/dev/iic-[1-12]``` represents I2C device on BLSP 1 to 12
   - UART: ```/dev/tty-[1-4]```  

*NOTE:* 
- The /dev/i2c-[0-11] numbering convention will be deprecated soon. Update your software to use the /dev/iic-[1-12] device numbering convention.
- Up to four UART devices are supported. Each UART device is associated with a BAM device.
Selecting any BLSP port as an SPI or I2C device is already supported in DspAL. The following section describes how to configure UART to BAM port mapping at runtime.  

### How it works
During boot time, the aDSP loads a BLSP configuration file to initialize the UART devices. To enable run time configuration, define the UART device to BAM port mapping in the file /usr/share/data/adsp/blsp.config.
The following is a sample blsp.config file showing the default board UART to BAM mapping:  
```
tty-1 bam-9 2-wire  
tty-2 bam-8 2-wire  
tty-3 bam-6 2-wire  
tty-4 bam-2 2-wire
```
Each line begins with the UART device name "tty-[x]" where x is between 1 and 4 inclusive. The device name is followed by a space and bam-[y] where y is between 1 and 12 inclusive. To indicate that the UART should only use two wires, one to receive and the other to transmit, include the text "[2-wire]" at the end of the line.  

Note the following:
- Not all four UART devices need to be configured. Developers may enable fewer than four UART devices.
- In case the config file does not exist or loading the file fails due to the incorrect format, the default settings listed above will be used.
- After creating and editing the ```/usr/share/data/adsp/blsp.config``` file, it is recommended to set the file to read-only mode.
- If the two-wire designation is not included, the UART defaults to using four wires: receive data, transmit data, clear to send (CTS), and receive to send (RTS)
This will cause a problem for any other type of I/O on the same connector, since the pins will be configured as RTS and CTS signals.  

#### Sample logs  
During aDSP boot time, the status of the run time configuration operation is shown in the mini-dm logs.
The following is a sample log when the run time configuration was successful and the /usr/share/data/adsp/blsp.config was successfully loaded.
```
QDSP6/High00:04:21.725 blsp_config.c 00189 loading BLSP configuration
QDSP6/High00:04:21.726 blsp_config.c 00208 opened /usr/share/data/adsp/blsp.config
QDSP6/High00:04:21.726 blsp_config.c 00170 UART tty-1: BAM-6
QDSP6/High00:04:21.726 blsp_config.c 00170 UART tty-2: BAM-9
QDSP6/High00:04:21.726 blsp_config.c 00170 UART tty-3: BAM-2
QDSP6/High00:04:21.726 blsp_config.c 00170 UART tty-4: BAM-8
QDSP6/High00:04:21.727 main.c 00268 BLSP configuration loaded
```
The following is a sample log when the run time configuration failed and the /usr/share/data/adsp/blsp.config file did not exist. In this case, the aDSP uses default UART to BAM mappings.
```
QDSP6/Medium00:00:43.775 file.c 00162 HAP:159:open file blsp.config in mode rb
QDSP6/High00:00:43.776 file.c 00167 HAP:159:unable to open the specified file path
QDSP6/Fatal00:00:43.776 blsp_config.c 00204 failed to open /usr/share/data/adsp/blsp.config
QDSP6/Fatal00:00:43.776 main.c 00261 QDSP6 Main.c: blsp_config_load() failed
QDSP6/High00:00:43.776 blsp_config.c 00035 Loaded default UART-BAM mapping
QDSP6/High00:00:43.776 blsp_config.c 00043 UART tty-1: BAM-9
QDSP6/High00:00:43.776 blsp_config.c 00043 UART tty-2: BAM-6
QDSP6/High00:00:43.776 blsp_config.c 00043 UART tty-3: BAM-8
QDSP6/High00:00:43.776 blsp_config.c 00043 UART tty-4: BAM-2
```

## Camera
### Take a picture
The following is required to take a picture:
   - The ```camera::ICameraDevice::takePicture()``` function triggers the capture of a snapshot.  
   - The caller must wait for the snapshot frame to be delivered before additional pictures can be taken.  
   - The output snapshot image is delivered in the ```camera::ICameraListener::onPictureFrame()``` callback. This function should awaken the waiting thread.  
   - The snapshot dimensions and format can be set using the corresponding parameters defined in ```camera::CameraParams```.

### Sequence diagram
Below is the call flow sequence diagram for a typical camera use case:
![Image](./camera-sequence-diagram.jpg?raw=true)

### Use cases
For specific use cases of the API, such as stereo camera streaming, raw preview, and high framerate support, refer to the application note in the example test application. This application is located at: https://source.codeaurora.org/quic/le/platform/hardware/qcom/camera/tree/libcamera?h=LNX.LER.1.2.

## Acronyms and abbreviations
- BAM: Bus access manager
- BLSP: BAM low-speed peripheral

## Additional references
Please refer to the following pages and documents for additional information:
- User Guide and Developer Guide documents at: http://support.intrinsyc.com/projects/snapdragon-flight/documents
- Information on pinouts, connectors and peripherals: http://dev.px4.io/hardware-snapdragon.html
- Useful information and How-To's: http://dev.px4.io/advanced-snapdragon.html
