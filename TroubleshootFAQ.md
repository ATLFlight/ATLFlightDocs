# Troubleshooting and FAQs
This page provides answers to common questions, solutions to common problems and other how-tos.

1. [Revive board that will not boot](revive-board-that-will-not-boot)
1. [Install OpenCV](install-opencv)

## Revive a board that will not boot
If your Snapdragon Flight board does not boot up, you may be able to recover it with a *serial console adapter* board (see [here](http://shop.intrinsyc.com/products/snapdragon-flight-dev-kit) for details). Use the following procedure to recover the Snapdragon Flight board:  
  1. Disconnect power and USB cable(s) from the Snapdragon Flight and debug boards.  
  2. Plug the Snapdragon Flight board into the debug board.  
  3. Enable (insert jumper) J2 (FASTBOOT) of the debug board.  
  4. Connect the debug board’s J4 to the computer’s USB port, using the provided USB cable, and open a serial terminal (CuteCom or Putty).  
  5. Power up the debug board.  
  6. Connect the Snapdragon Flight board to the computer via a USB cable.  
  7. Validate the connection by typing “fastboot devices” on a command prompt or terminal window. You should now see the device-specific random number displayed.  
  8. You should be able to flash the platform software images at this point.

To boot the board normally again, perform the following steps:  
  1. Unplug the power supply and USB cable.  
  2. Remove the jumper from J2 on the serial console debug board.  
  3. Power up the Snapdragon Flight board and connect the board to the computer via a USB cable.  
  4. Validate the connection by typing “adb devices” on a command prompt or terminal window.

## apt-get does not work

Ensure that the Snapdragon Flight board is configured in [Station mode](http://dev.px4.io/advanced-snapdragon.html#station-mode) and connected to the Internet: 

apt-get on Snapdragon Flight is broken. We need the following work-around to install packages:

Uninstall certain on-board packages in order to get apt-get to work. 
```apt-get install -f```

Install the package(s) that you want using apt-get.

Reinstall the previously-uninstalled packages as follows:
```shell  
mkdir temp-mount  
mount /dev/mmcblk0p12 temp-mount/  
cd temp-mount/ qrlPackages/  
dpkg -i fastcv-internal_1.0-r0_armhf.deb  
dpkg -i mm-camera-lib-prebuilt_1.0 
```

## Install OpenCV

The standard way to install OpenCV is: ```apt-get install --fix-missing libopencv-highgui-dev```

Since apt-get on Snapdragon Flight is broken, we need to use the following work-around:

Uninstall certain uninstalled packages in order to get apt-get to work, and then install OpenCV.
```shell  
apt-get install -f  
apt-get -o Dpkg::Options::="--force-overwrite" install libglib2.0-dev  
apt-get install --fix-missing libopencv-highgui-dev
```

Reinstall the previously-uninstalled packages as follows:
```shell
mkdir temp-mount  
mount /dev/mmcblk0p12 temp-mount/  
cd temp-mount/ qrlPackages/  
dpkg -i fastcv-internal_1.0-r0_armhf.deb  
dpkg -i mm-camera-lib-prebuilt_1.0 
```
