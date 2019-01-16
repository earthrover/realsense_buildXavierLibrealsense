# buildLibrealsense2Xavier
Build librealsense 2.0 library on the NVIDIA Jetson AGX Xavier Developer Kit. Intel RealSense D400 series cameras.

This is for version L4T 31.1 (JetPack 4.1.1), librealsense v2.17.1.

January 2019

In order for librealsense to work properly, the kernel Image must be rebuilt and patches applied to the UVC module and some other support modules. In addition, for support of the extra features of the D435i camera such as the IMU, extra modules must be built.

Because the AGX Xavier runs kernel version 4.9 and librealsense does not support that kernel revision directly, custom patches have been constructed based on the patches in librealsense. These are located in the 'patches' directory.

In order to build and install the patched kernel Image and modules:

$ ./buildPatchedKernel.sh

The kernel Image on the Jetson AGX Xavier is signed. This means that a generated Kernel Image cannot be used directly on the Jetson without having an application sign the file. Currently, the signing application is an x86 app running on the host machine. The easiest way to apply the Image to the Jetson is to use the host machine that flashed the Jetson to sign and flash the patched kernel. 

This is a two step process. 
First make a copy of the orignal Image file as a backup. The Image file is located on the host in the JetPack Xavier/Linux_for_Tegra/kernel directory. Then, replace the JetPack Xavier/Linux_for_Tegra on the host with the Image file generated by buildPatchedKernel.sh (located in the 'image' directory) on the Jetson. You can use scp or removable media to copy the file from the Jetson to the host.

The second step is to flash the kernel onto the Jetson AGX Xavier. Place the Xavier in recovery mode, connected to the host machine using the supplied USB C to USB A cable. This is the same physical connection and procedure as when flashing the Jetson with JetPack. Then from the Xavier/Linux_for_Tegra/ directory:

$ sudo ./flash.sh -k kernel jetson-xavier mmcblk0p1

This should flash the Jetson. The -k parameter instructs the flash script to only copy the kernel Image. You should then replace the modified Image with the stock Image on the host.

Move back to the Jetson and navigate to this repositories directroy. 

You may remove all the kernel source using the provided convenience script 'removeAllKernelSources.sh'.

You are then ready to install librealsense 2.

Run the convenience script:

$ ./installLibrealsense.sh

This will build the librealsense libraries and examples. The examples will be placed in /usr/local/bin.

Application Patching

Currently there are issues with the realsense-viewer application. The first issue is that sometimes when using large frame sizes, incomplete frames are transmitted. The second issue is that one of the processes tends to block. There are two work around patches provided here as work arounds. The patches may be applied with:

$ ./patchApplication.sh

Note that you will need to recompile the library and application for these to take effect.

Release Notes:

January, 2019

* Initial Release
* JetPack 4.1.1
* L4T 31.1




