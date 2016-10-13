# Snapdragon Flight User Guide
This document is a partial user guide for Snapdragon Flight.

# Table of Contents
1. [Logging](#logging)
   - [mini-dm](#mini-dm)
2. [Flight Control](#flight-control)
   - [WiFi control](#wifi-control)
      - [UAV setup](#uav-setup)
      - [View FPV video stream](#view-fpv-video-stream)
      - [Spektrum transmission over WiFi](#spektrum-transmission-over-wifi)
      - [Setup QGroundControl application](setup-qgroundcontrol-application)
    - [RC radio control](rc-radio-control)
 
## Logging
### mini-dm
The mini-diagnostic monitor (mini-dm) is used to get debug messages from the DSP (including the FastRPC application) using the following procedure:

1. Download and install  the [Hexagon SDK](https://developer.qualcomm.com/software/hexagon-dsp-sdk/tools) on your host machine.
2. Navigate to the mini-dm executable location:
   * Linux: ``` ${HEXAGON_INSTALL_HOME}/Qualcomm/Hexagon_SDK/3.0/tools/debug/mini-dm/Linux_Debug ```
   * Windows: ```${HEXAGON_INSTALL_HOME}\Qualcomm\Hexagon_SDK\3.0\tools\debug\mini-dm\WinNT_Debug```
3. Connect the USB cable from the Snapdragon Flight board to the host machine where the mini-dm is installed.
4. Run the mini-dm using the following command: ``` ./mini-dm ```(Linux) OR ```mini-dm.exe``` (Windows)
5. Run the FastRPC or other application that loads the aDSP module. For more information, see the Hexagon SDK documentation located at ```${HEXAGON_INSTALL_HOME}/Qualcomm/Hexagon_SDK/3.0/docs/index.html```
6. For more information on mini-dm, see this [FAQ](https://developer.qualcomm.com/forum/qdn-forums/mobile-technologies/multimedia-optimization-hexagon-sdk/toolsinstallation/27111).

## Flight Control
Flight control commands are transmitted over a Wi-Fi or RC communication link.
### WiFi control
The Wi-Fi communication link can transmit commands to the unmanned aerial vehicle (UAV). 
For information on installation of antennas for Wi-Fi, see Qualcomm Snapdragon Flight User Guide (80-H9581-1).
#### UAV setup
By default, the system should come up in SoftAP mode. If the default is not set, this configuration can be enable with the following steps:
1. Enable AP mode and reboot (see TODO for additional information).
2. The AP mode defaults with automatic channel selection (ACS), which selects a 2.4 GHz channel with the least interference. If this is not the desired provisioning, this can be configured manually. Scan for other APs in the vicinity and configure SoftAP with a channel unlikely to suffer from interference with other devices. See TODO for additional information.
3. Once SoftAP mode is enabled, run the following command (through adb or serial console to the UAV) to determine its server set identifier (SSID):
```/usr/local/qr-linux/wificonfig.sh –g```
4. When the PX4 flight stack is installed, this should include provisioning for MavLink communication to allow commands to be received and used by the flight stack. See TODO for additional details.
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
