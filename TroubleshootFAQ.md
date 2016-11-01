# Troubleshooting and FAQs
This page provides answers to common questions, solutions to common problems and other how-tos.

1. [Revive board that will not boot](#revive-board-that-will-not-boot)
1. [Serial console adapter board](#serial-console-adapter-board)
1. [apt-get does not work](#apt-get-does-not-work)
1. [Install OpenCV](#install-opencv)
1. [WiFi network connection](#wifi-network-connection)  
1. [Wired ethernet connection](#wired-ethernet-connection)
1. [USB3 cable](#usb3-cable)
1. [Camera image is rotated](#camera-image-is-rotated)
1. [USB-to-serial debug cable](#usb-to-serial-debug-cable)
1. [Debugging ADB problems](#debugging-adb-problems)

## Revive board that will not boot
If your Snapdragon Flight board does not boot up, you may be able to recover it in one of the following ways:

### Factory reset  
This is documented in the "Factory reset" chapter of the Snapdragon Flight User Guide: http://support.intrinsyc.com/documents/125  

### Fastboot mode
with a *serial console adapter* board (see [here](http://shop.intrinsyc.com/products/snapdragon-flight-dev-kit) for details). Use the following procedure to recover the Snapdragon Flight board:  
  1. Disconnect power and USB cable(s) from the Snapdragon Flight and debug boards.  
  2. Plug the Snapdragon Flight board into the debug board.  
  3. Enable (insert jumper) J2 (FASTBOOT) of the *serial console adapter* board.  
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

*NOTE:* If none of the above options work, please contact [Intrinsyc support](https://www.intrinsyc.com/contact-support) to get it reflashed.

## Serial console adapter board  
The serial console adapter board and cable are available as part of the [Snapdragon Flight developer's edition](http://shop.intrinsyc.com/collections/product-development-kits/products/snapdragon-flight-dev-kit). This allows the board to be accessed through a USB connector for debugging purposes. It also provides jumpers to force the board in fastboot and other special modes if necessary.

Some customers may have received an older deprecated version of the serial console adapter board does not include the fastboot jumper support. Please look at the MCN printed on the board to identify it:
  - 25-H9563 - Older serial console board, it does not include the fastboot jumper pins
  - 25-H9916 - New serial console board, it includes the fastboot jumper pins (J4)  

If you have the older version, please contact Intrinsyc to get the newer Serial Console Adapter board.

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

## WiFi network connection  
To connect Snapdragon Flight board to a WiFi router on the network, please follow these instructions (see Station mode under Wifi-settings):  
http://dev.px4.io/advanced-snapdragon.html

## Wired ethernet connection  
The board can be connected to the network over a wired connection via the OTG USB port by using a USB Ethernet adapter (such as http://www.apple.com/shop/product/MC704LL/A/apple-usb-ethernet-adapter) along with a USB 2.0 Female to Micro USB Male OTG adapter (such as http://www.rakuten.com/prod/268480078.html).

## USB3 cable  
Any standard USB micro-B cable may be used with the board. But in order to get USB 3.0 SuperSpeed functionality, one requires a MICRO-A PLUG TO STD-A cable (such as the Amphenol RUB30-0075 http://www1.amphenol-ast.com/v3/en/product_view.aspx?id=189).

## Camera image is rotated  
On some of the newer boards from Intrinsyc, the 4K camera images are rotated by 90 degrees. This is a known issue. A software resolution is being worked on and will be provided shortly.

## USB-to-serial debug cable
The USB-to-Serial Debug Cable that plugs into the Serial Console Adapter is the TTL-232R-3V3 part from FTDI modified to be 4-pins and keying added to prevent incorrect insertion. For more information, see http://www.ftdichip.com/Support/Documents/DataSheets/Cables/DS_TTL-232R_CABLES.pdf.

## Debugging ADB problems
  1. Verify that the Snapdragon Flight target is listed when you type ```adb devices``` on the host computer?
  2. Do ```ls -l /etc/udev/rules.d``` and check for the existance of the 51-android.rules file.
  3. Verify that the following lines are present in your ```/etc/udev/rules.d/51-android.rules``` file? If not, add them and try again.
```
#for Device adb interface
SUBSYSTEM=="usb", ATTR{idVendor}=="05c6", MODE="0666", GROUP="plugdev"
#for Fastboot bootloader interface
SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", MODE="0666", GROUP="plugdev"
```
  4. Do ```lsusb``` on the host PC after the Snapdragon Flight board is connected and powered up. You must see an entry for the target (usually listed as ```Qualcomm, Inc. Qualcomm HSUSB Device```).
  5. Do ```adb kill-server``` and then ```adb start-server``` and try again?
  6. If none of this works, verify that adb is functional on your host computer by connecting a phone or other device.
