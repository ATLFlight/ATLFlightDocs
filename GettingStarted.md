# Getting Started on Hexagon DSP Application Development

These instructions are for using DSPAL and DriverFramework with the Hexagon Toolchain on x86_64 Linux.

## Installations
### SDK
Install the Hexagon™ SDK using the instructions at [Development Environment Setup](DevEnvSetup.md).

### cmake
If you use Ubuntu 14.04, upgrade cmake to 3.4.3 or higher, and add it to the PATH.
   ```
   cd ~/Downloads
   wget https://cmake.org/files/v3.4/cmake-3.4.3-Linux-x86_64.sh
   chmod a+x cmake-3.4.3-Linux-x86_64.sh
   ./cmake-3.4.3-Linux-x86_64.sh
   sudo mv cmake-3.4.3-Linux-x86_64 /opt
   ```
   In your shell startup script, add
   ```
   if [ "x`echo $PATH | grep '/opt/cmake-3.4.3-Linux-x86_64/bin'`" = "x" ]; then
   PATH=$PATH:/opt/cmake-3.4.3-Linux-x86_64/bin
   fi
   export PATH=$PATH
   ```
### Other packages
```
sudo apt-get install python-empy
sudo apt-get install python-pip
sudo pip install catkin_pkg
```

## All Done. What Next?

You have installed all the prerequisites to build code for the Hexagon DSP. Try the [HelloWorld](HelloWorld.md)
program to test running a program on the DSP.
