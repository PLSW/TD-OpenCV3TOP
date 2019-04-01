# TD-OpenCV3TOP
## Touchdesigner OpenCV3 C++ TOP for FaceDetect

_**Contributors**_

- **Da Xu** - *Development and Documentation*
- **[Greg Finger of PLGRM Visuals](https://github.com/gregfinger)** - *Development, Demo Projects, Testing and Foreman*



_**The Binary Release has been built against:**_
- **Stable** Touchdesinger 2018.27910
- **Experimental** Touchdesigner 2019.12330
- OpenCV version 3.4.4 for **Mac** and 3.4.5 for **Windows**.


_**Functionality/Purpose:**_

This plugin adds the OpenCV FaceDetect functionality to TD via a C++ TOP. 


Though OpenCV has already been integrated into TD, it's done via Python, which is not terribly convenient when you have to deal with streams of data. This plugin now allows you to pipe data from any TOP straight into the OpenCV algorithms and spit out data in a way that can be acted upon immediately. It makes the whole process much more streamlined and faster.


## Release Notes v1.5 3/31/2019
Yes, we skipped over a few minor versions. But since we added quite a few things to this release, we feel we deserve the 1.5.


- The plugin is now **cross-platform**. We now have binary releases for both **Mac** and **Windows**, along with the necessary code and project files to build it on your own, if you so choose.
- The plugin now supports both Touchdesigner **Stable** and **Experimental** branches. The Experimental branch has implemented a new way of dealing with C++ plugins, and it allows users to utilize C++ plugins without a Commercial or Pro License. Make sure you use the correct binary release for the branch of TD you are using.
- Greg Finger has created a demo toe file for the Experimental branch.
- Extensive updates have been made to the documentation. We now include instructions on how to build OpenCV on Mac, as well as build instructions for the plugin.
- scaleFactor has been exposed to the user via parameters.
- The plugin now forces 32 bit float data format.




## How Does It Work?

- Once face(s) has been detected, the detection results are passed out via **Info Dat AND Pixel Packing**.
Results are packed into the output frame (RGBA32Float) in the following manner:

- The 1st Pixel's R contains how many faces have been detected. 

- I use the RGBA channels of the subsequent pixels to store (x, y, width, height) of the detected face(s), in the order that they have been detected.


- There is a Sanity Check toggle that let's you see exactly what the detector sees. *This feature is only available on Windows.*

- If no face was found, the first pixel will be 0 across the board.


Please check Greg Finger's demo Touchdesigner project to see how to use this C++ TOP. AGAIN, you WILL NEED a Commercial or Pro license if you are working with the Stable branch of Touchdesigner.

## Additional Notes
I have exposed most of the parameters of the detectMulti call as custom parameters. Min and Max search sizes have been expressed as a ratio in relation to the input height. You can drop an Info CHOP down to see the exact size of each in pixels.



Try different cascade files, they do effect result and speed. You can find additional cascade files in the OpenCV3 distro.



## Usage
See Greg Finger's demo projects for usage, particularly on how to deal with the output of the plugin.


### Binary Release
A quick word on **Windows** vs. **Mac**

Our binary release for **Windows** does not contain the necessary OpenCV DLL to run the plugin. You will need to handle this part yourself. Please refer to the instructions below. It's quite painless. We chose this path due to the fact that this particular task is rather trivial for the user. We try not to include external libraries in our binary release unless absolutely necessary. 

Such is the case with **Mac**. Since Apple has decided to hide the standard LD_LIBRARY_PATH from the user, getting the plugin to see the correct dynamic library has become a pain. On top of that, OpenCV does not provide a binary release of their library for Mac. That's why we have decided to package everything together into a single unit. The amount of work required to get everything working on **Mac** has exceeded our standards for convenience. 



#### - Stable Touchdesigner Build
You will load the plugin the same as any other Cplusplus plugin. 

**Windows** users will need to copy and paste the OpenCV world DLL into the same directory as the binary release DLL. 

**Mac** Nothing to do.


#### - Experimental Build
Please follow the instructions from Touchdesigner Wiki on [how to install Custom OPs](https://docs.derivative.ca/Experimental:Custom_Operators) 

**Windows** users will need to copy and paste the OpenCV plugin into the same directory as the binary release DLL. 

**Mac** Nothing to do. 



#### Windows-specific Instructions for using the plugin
You need to put the opencv world dll into the same directory as our binary release. 

Detailed instructions:

1. Goto [opencv.org's Releases page](https://opencv.org/releases.html), and download the Win Pack. 

2. Run downloaded exe file. It will ask you to extract the libs to a location of your choice. Find a nice and warm cozy place for it, then click extract.

3. Once the extraction is done, open up Explorer, and go to the dir that you have extracted to. 

4. There should be an opencv dir, go into it, and then goto build\x64\vc14\bin. 

5. Copy the opencv_world*version_number*.dll to the same directory as our binary release. NOTE: **DO NOT USE** THE opencv_world*version_number*d.dll. That's for debugging. It's hella slow.

opencv\build\etc contains the cascade files you need. The names are self-explanatory.



## Build Instructions

**Checklist before you run Build**

* OpenCV Library. This will include the headers, dynamic library for compiling, and DLL or dylib for running. **Windows** users can download binary releases from OpenCV's website. **Mac** users will have to download the source code and build their own, build instructions for the OpenCV library are below.
* Set the correct Headers search path. This will most likely be *opencv_install_dir/build/include* on **Windows** and *opencv_install_dir/include* on **Mac**. 
* Set the correct Library search path. This will most likely be *opencv_install_dir/build/x64/vc15/lib* on **Windows** and *opencv_install_dir/lib* on **Mac**.
* Set the dynamic library name. See specific instructions below.



### OpenCV Build Instructions for Mac

1. Download and install the [CMake binary distribution](https://cmake.org/download/)
2. Download and extract OpenCV source code, i.e. */Users/your_name/Documents/opencv.3.4.4* 
3. Open CMake, Click on *Browse Source*, and navigate to the directory where you have extracted the source code to, i.e. */Users/your_name/Documents/opencv.3.4.4*
4. Create a build directory. i.e. */Users/your_name/Documents/opencv.3.4.4/build*
5. Click on *Browse Build*, and navigate to the build directory you have created in Step 4.
6. Click *Conﬁgure*. Choose *Unix Makeﬁle* in the dialog that pops up. Then let CMake ﬁgure out the build conﬁgurations. 
7. Once Step 6 has ﬁnished, check **BUILD_opencv_world** option in the newly generated conﬁguration options.
8. Modify **CMAKE_INSTALL_PREFIX** to point to where you may want to install the build to. i.e. */Users/your_name/Documents/opencv_3.4.4*
9. Click *Generate* to generate the make ﬁles.
10. Open up Terminal, and cd into the build directory you have created in Step 4.
11. Enter the command *make*. This will build the library.
12. Once the build has ﬁnished successfully, enter the command *make install* to install the built binaries, headers, etc into the directory you have speciﬁed in **CMAKE_INSTALL_PREFIX**.
13. Done.


### Build Instructions for CV3 FaceDetect TOP
#### Mac OSX
*You will need to make the following modifications to the xcode project you downloaded from our repos.*

At this point, you should have a binary build of the OpenCV library of your choice on your local hard drive. *opencv_install_dir* from here and on will be used to refer to this location.

1. Add/Modify Framework reference to opencv library. 
    1. If you are using our xcode project, which is recommended, you will need to first delete the existing reference to OpenCV world dylib.
    2. Navigate to **General -> Linked Frameworks and Libraries**. *If you don't know how to get to this panel, please ask Google*
    3. Use Finder and navigate to your OpenCV install directory, under *lib*, there should be several world libraries. Two of them will be aliases or symlinks to the actual build. 
    4. Then from Finder, Drag and Drop (DO NOT use the plus sign, this will dereference the alias) the alias with only the major and minor version, i.e. *libopencv_world.3.4.dylib*. This ﬁle will be needed for linking and packaging.
    
2. Add Search Paths for Headers and Libraries 
    1. Navigate to **Build Settings -> Search Paths**. *Again, if you don't know how to get to this panel, please ask Google*
    2. Add *opencv_install_dir/include* to **Header Search Paths**
    3. Add *opencv_install_dir/lib* to **Library Search Paths**
    4. MAKE SURE you have added paths to both the Debug and Release schemes/profiles. You can check this by clicking on the triangle next to **Header Search Paths** and **Library Search Paths**

3. 
