# Camera Programming
This page provides information on writing and building code for Snapdragon Flight™ cameras.

1. [Pre-requisites](#pre-requisites)
1. [Sample Apps](#sample-apps)
  1. [camerad](#camerad)
1. [More information](#more-information)

## Pre-requisites
Follow the instructions in the [Getting Started](AppsGettingStarted.md) page to setup the SDK and build environment, and ensure that you can build and run the "Hello World" sample app.

## Sample Apps

### camerad
This is a simple camera daemon and client program for Snapdragon Flight™.

*Get the code*
```
# Top-level directory for the project
export ROOTDIR=/local/mnt/workspace/camerad-sample

mkdir -p $ROOTDIR
cd $ROOTDIR
git clone git://codeaurora.org/quic/le/platform/hardware/qcom/media -b LNX.LER.1.0
```
*Build*  
Follow the instructions below OR in the $ROOTDIR/media/readme.txt file.
```
cd media/camera-daemon 
make -f sdk_use.mk
```
*Documentation*
- Generate the doxygen documentation: ```cd doxy && make```
- Open the following file from a web browser: ``` $ROOTDIR/media/camera-daemon/doxy/html/index.html ``` to view and browse the documentation.

*Run*  
Push the binaries to target
```
adb push camclient /home/linaro
adb push camerad /home/linaro
```
Navigate to the User Guide in the above doxygen documentation and follow the instructions to start and run the ```camerad``` and then the ```camclient```.

## More information
Snapdragon Flight platform developer's edition users may see this document for more information [Snapdragon Camera Linux Camera Interface Specification](http://support.intrinsyc.com/documents/143).
