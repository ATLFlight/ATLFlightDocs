# Hello World

The following instructions describe how to compile and run a hello world program on the Hexagon DSP.

This guide assumes that the steps in [Getting Started](GettingStarted.md) have already been completed.

Clone the [HelloWorld project](https://github.com/ATLFlight/HelloWorld) and build it

```
host%> git clone https://github.com/ATLFlight/HelloWorld
host%> cd HelloWorld
host%> make
```

Connect the target board to the host computer via the ADB usb port. Then load the ARM and DSP portions
of the HelloWorld application.

```
host%> make load
```

Run mini-dm to see the output from the DSP

```
host%> ${HEXAGON_SDK_ROOT}/tools/mini-dm/Linux_Debug/mini-dm
```

Open an new shell and run the application on the target

```
host%> adb shell
target#> /home/linaro/helloworld
```

You should see the following output on the terminal running mini-dm:

```
DMSS is connected. Running mini-dm...
<warnings you can ignore>
[08500/00]  40:27.640  HAP:12365:Hello World  0006  helloworld_qurt.c

```
