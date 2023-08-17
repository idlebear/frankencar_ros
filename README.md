# FrankenCar ROS configuration and launch files

All files related to the configuration and launch of the FrankenCar can be found here.  Mostly these link into the F1Tenth ecosystem, launching the required components to produce an autonomous car.  

## Building Libraries

The various libraries can be challenging to build, or at least, find the correct instructions to build.  As new modules are added, they'll be documented and built here

### Jetpack

Install Jetpack 4.6.3 (32.7.3).  That's the most current version that has orbitty drivers, and is easily modded to work with installing realsense.

### Librealsense

Decent instructions for building and installing can be found here.  For things to work well, you need the native drivers.

First edit *./scripts/patch-realsense-ubuntu-L4T.sh* and search for 4.6.1.  Modify the condition to allow 4.6.3
```
  "32.4.4" | "32.5" | "32.5.1" | "32.6.1" | "32.7.1" | "32.7.3" )
    PATCHES_REV="4.4.1" # JP 4.4.1, 32.7.1 is JP 4.6.1
    ;;
```
Save, then run the script.  It should work as expected.  Note that you can not use 4.6.2 as nVidia appears to have disappeared the release files and the script will break.

Then build everything using
```
sudo apt-get install git libssl-dev libusb-1.0-0-dev pkg-config libgtk-3-dev -y
./scripts/setup_udev_rules.sh
mkdir build && cd build
cmake .. -DBUILD_EXAMPLES=true -DCMAKE_BUILD_TYPE=release -DFORCE_RSUSB_BACKEND=false -DBUILD_WITH_CUDA=true && make -j$(($(nproc)-1)) && sudo make install
```
You may also want to include the python wrappers, although with this current release only python3 is supported.

