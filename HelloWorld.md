# Hello World

The following instructions describe how to compile and run a hello world program on the Hexagon DSP.

This guide assumes that the steps in [Getting Started](GettingStarted.md) have already been completed.

Clone the [HelloWorld project](https://github.com/ATLFlight/HelloWorld) and build it

```
host%> git clone https://github.com/ATLFlight/HelloWorld
```
## Build using QURT_BUNDLE

```
host%> cd HelloWorld/bundle
host%> make
```
Connect the device via ADB and make sure it can be found.
```
adb devices
```
You should see something like:
```
$ adb devices
List of devices attached 
997e5d3a	device
```

Then load the ARM and DSP portions of the HelloWorld application.
```
host%> make load-bundle
```

Run mini-dm to see the output from the DSP

```
host%> ${HEXAGON_SDK_ROOT}/tools/debug/mini-dm/Linux_Debug/mini-dm
```

Open an new shell and run the application on the target. Note that when using
QURT_BUNDLE, the apps proc app has "\_app" appended to differentiate the
helloworld library and the helloworld app, otherwise cmake is confused.

```
host%> adb shell
target#> /home/linaro/helloworld
Asking DSP to say hello
```

You should see the following output on the terminal running mini-dm:

```
DMSS is connected. Running mini-dm...
<warnings you can ignore>
[08500/00]  40:27.640  HAP:12365:Hello World  0006  helloworld_qurt.c

```

## Build using separate builds for apps proc and aDSP proc

```
host%> cd HelloWorld
host%> make
```
Connect the device via ADB and make sure it can be found.
```
adb devices
```
You should see something like:
```
$ adb devices
List of devices attached 
997e5d3a	device
```

Then load the ARM and DSP portions of the HelloWorld application.
```
host%> make load
```

Run mini-dm to see the output from the DSP

```
host%> ${HEXAGON_SDK_ROOT}/tools/debug/mini-dm/Linux_Debug/mini-dm
```

Open an new shell and run the application on the target

```
host%> adb shell
target#> /home/linaro/helloworld
Asking DSP to say hello
```

You should see the following output on the terminal running mini-dm:

```
DMSS is connected. Running mini-dm...
<warnings you can ignore>
[08500/00]  40:27.640  HAP:12365:Hello World  0006  helloworld_qurt.c

```
