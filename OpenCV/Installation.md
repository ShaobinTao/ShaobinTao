## Purpose
When I was trying to build OpenCV(**4.1.0** as of writing) from source and install it on **Ubuntu 14.04 LTS**, the steps were pretty smooth. But when I tried to run a helloworld sample, it reported a linking problem as in Appendix 1. I spent a couple of hours, asked two expert coworkers and still did not figure out why. Therefore, I want to document the steps for those who run into same issues later. First of all, the takeaways:

* OpenCV 4.1.0 does not work well Ubuntu 14.04 LTS. I tried [OpenCV 3.2.0](https://github.com/opencv/opencv/releases/tag/3.2.0) and it worked for me.
* ld cache needs to be refreshed in order to use locally built OpenCV libs.
* The HelloWorld sample is for C/C++ users.

## Detailed Steps
The main steps I found are from [Open CV docs](https://docs.opencv.org/4.0.1/d7/d9f/tutorial_linux_install.html). For completeness, I copied/pasted some steps here.
* Install the dependency packages documented at https://docs.opencv.org/4.0.1/d7/d9f/tutorial_linux_install.html.
* Download [OpenCV 3.2.0 source code](https://github.com/opencv/opencv/archive/3.2.0.zip). Note "git clone" would work also. Assume the source code is unziped to ~/opencv
* Create a temp folder build
```
cd ~/opencv
mkdir build
cd build
```
* Run cmake-gui to configure the build settings
```
cmake-gui
```
Configure the build settings as needed. For persons who choose to build from source, likely, we want a debug build which we can debug. Some main settings of interest are:
* "Where is the source code": ~/opencv
* "Where to build the binaries": ~/opencv/build
* Press "Confgure" button
* Press "Generate" button
* In "Search"" editbox, change settings below as needed
  * CMAKE_BUILD_TYPE=Debug
  * search "debug" and change those needed
  * search "opt" (optimization) and change those needed
  * search "pkg" (for package configuration pc file generation, available on 4.1.0, not on 3.2.0)
  * search "opengl" "opencl" "cuda" etc

* Build the binaries
```
~/opencv/build/make -j16
// -j16 means run 16 jobs in parallel. The number depends on how many CPU cores your computer has.
```

* Install the binaries to the right locations
```
sudo make install
```

* Update ld cache so your programs can find the new libs (**this step is not in OpenCV doc**)
```
sudo make install
```

## HelloWorld example
For quick testing, download [Makefile](https://github.com/ShaobinTao/ShaobinTao.github.io/blob/master/OpenCV/Makefile), [hello.cpp](https://github.com/ShaobinTao/ShaobinTao.github.io/blob/master/OpenCV/hello.cpp) to your ~/opencv directory. Run
```
opencv$ make
```
Hooray, you can test OpenCV functions locally now.


## Appendix
1. imshow linking error 
```
g++ -o hello -Wall -std=c++0x -I/usr/local/include/opencv4 -L/usr/local/lib -L/usr/lib hello.cpp -lopencv_core -lopencv_highgui -lopencv_imgcodecs -lopencv_imgproc -lopencv_video -lopencv_calib3d -lopencv_dnn -lopencv_features2d -lopencv_flann -lopencv_gapi -lopencv_photo -lopencv_stitching
/tmp/cc2bXDvL.o: In function `main':
hello.cpp:(.text+0x153): undefined reference to `cv::imshow(std::string const&, cv::_InputArray const&)'
collect2: error: ld returned 1 exit status
make: *** [TARGET] Error 1
```

