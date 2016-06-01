# DSPAL

The DSP Abstraction Layer (DSPAL) provides a mostly POSIX compliant
interface to ease the porting of code from Linux or another posix
compliant OS to the Hexagon DSP.

## Using DSPAL

The header file for DSPAL are maintained at: http://github.com/ATLFlight/dspal

Support for DSPAL is built into the DSP image for Snapdragon Flight.
These files are available as part of the software available from Intrinsyc.

The DSP files are located in /lib/firmware in the rootfs installed on the
Snapdragon FLight board.

```
# cd /lib/firmware
# ls adsp*
adsp.b00  adsp.b02  adsp.b04  adsp.b06  adsp.b08  adsp.b11  adsp.mdt
adsp.b01  adsp.b03  adsp.b05  adsp.b07  adsp.b10  adsp.b12  adsp_ver_1.0-r0
```

New versions of DSPAL may require updated DSP images.

DriverFramework includes dspal as a submodule so you generally do not need to
build dspal directly unless you wish to run the test SW.

## Building the test SW

```
git clone https://github.com/ATLFlight/dspal
cd dspal/test
make
make load
```

### Checking the DSP SW version

There is a utility to check the version of the DSP SW.

Connect the Snapdragon Flight board via micro USB connector for ADB.

Then get a shell on the device:

```
adb shell
# cd /home/linaro
# ./version_test
# exit
```

Amongst the debug output you should see something like:

```
version: DSPAL_VERSION_STRING=DSPAL-1.0.1.0001
build date: BUILD_DATE_STRING=Feb  2 2016
build time: BUILD_TIME_STRING=19:10:21
```

### Running the DSPAL Unit Tests

Part of the test SW is a DSPAL unit test. You will need to run mini-dm
to see the output from the DSP.

In a new terminal, run:
```
${HEXAGON_SDK_ROOT}/tools/debug/mini-dm/Linux_Debug/mini-dm
```

To run the unit test:

```
adb shell
# cd /home/linaro
# ./dspal_tester
# exit
```
