---
title: Prismatic Packaging 
subtitle: Instructions for creating Prismatic packages from source
---


## Mac OS X

*Currently, Macs do not contain NVIDIA GPUs, so there is only a CPU version of the application bundle*


#### Building Mac OS X bundle with Qt

##### Building with Qt frameworks
In more recent version of Prismatic, CMake is configured to generate an app bundle. After you have built this, run the script "mac_build.sh" from within the build directory (i.e. run `../mac_build.sh`). This script handles the complicated process of copying the necessary Frameworks into the app bundle and reconfiguring the names of their library paths to point to the bundled version.

##### Building with static Qt (outdated)
Deploying on Mac requires a statically built version of the Qt libraries with `./configure -static` (see [here](http://doc.qt.io/qt-5/osx-deployment.html)). You will then build `prismatic-gui` through the Qt project `Qt/prismatic-gui.app` by invoking `qmake` (from the statically build version of Qt5). For example on my system I would invoke `/Users/ajpryor/Qt5/qtbase/bin/qmake` followed by `make -j8`, which results in `prismatic-gui.app` within the `Qt` folder. You can check that the binary is statically linked with `otool -L prismatic.gui.app/Contents/MacOS/prismatic-gui`. If the output from this command does not contain any Qt libraries, then you have successfully statically linked them.

From an existing version of the prismatic-gui.app bundle, simply copy the latest `prismatic-gui` into Contents/MacOS and the rest of the bundle configuration (such as the icons) should be taken care of

Should you need to remake the bundle, it should contain the following directory structure  

Contents/  
&nbsp;&nbsp;&nbsp;&nbsp;Info.plist  
&nbsp;&nbsp;&nbsp;&nbsp;MacOS/  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;prismatic-gui  
&nbsp;&nbsp;&nbsp;&nbsp;Resources/  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;prismatic-gui.icns

Where prismatic-gui.icns was made following [this blog post](https://blog.macsales.com/28492-create-your-own-custom-icons-in-10-7-5-or-later) using Inkscape to generate the images and `iconutil` to convert the iconset to a .icns.  

 Info.plist is a basic XML file with the following contents
