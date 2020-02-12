# Caffe-Install-macOS-10.15-Catalina
Caffe 1.0 version install on macOS 10.15 (Catalina) for cpu-only version.  
I refered to following guides which greatly helped to successfully install Caffe on my macOS.  
This repository aims to make Caffe installtaion easier by properly mixing advices from the guides.  

1. https://www.dazhuanlan.com/2019/08/15/5d5514f5efcdc/
2. https://github.com/reuank/caffe-macos-catalina

# Installation Environment
- macOS: 10.15.3
- Homebrew: 2.2.5
- Python: 3.7.6_1
- OpenCV: 4.2.0_1
- boost-python3: 1.72.0

# Steps
1. If Xcode is not installed, you need to download Xcode from App Store.

2. If Homebrew is not installed, type following command in Terminal.
```
 /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
 
3. Type following command in Terminal.
```
 sudo chown -R $(whoami) $(brew --prefix)/*
```
 
4. If python3 is not installed, install python3 by typing following commands in Terminal.
```
 brew upgrade
 brew update
 brew install python
```

5. Install required packages for Caffe.
```
 brew install -vd snappy leveldb gflags glog szip lmdb
 brew install openblas
 brew install hdf5 opencv
 brew install boost boost-python3
 brew install automake libtool
```
 
6. Install protobuf. Latest version of protobuf can cause errors, thus 3.5.1 version used.
```
 cd ~/Downloads 
 wget https://github.com/protocolbuffers/protobuf/archive/v3.5.1.zip
 unzip v3.5.1.zip
 cd protobuf-3.5.1
 ./autogen.sh
 ./configure
 make
 make install
```

7. Install Caffe.
```
 cd ~/
 git clone https://github.com/BVLC/caffe.git
 cd caffe/
 cp Makefile.config.example Makefile.config
```

8. Modify common.h file to work with OpenCV 4 version.
```
 open ./include/caffe/
```
 
Open common.h file with text editor and copy-and-paste below command at line 72.
```
 // Supporting OpenCV4
 #define CV_LOAD_IMAGE_GRAYSCALE cv::IMREAD_GRAYSCALE
 #define CV_LOAD_IMAGE_COLOR cv::IMREAD_COLOR
```
 
9. Modify Makefile.config.
```
 open ~/caffe/Makefile.config
```

Copy all lines in Makefile.config file in this repository to your Makefile.config file.  

**Note**  
If your Python version is different with 3.7.6_1, you need to revise these two lines in Makefile.config.  
Change the ** ** parts accordingly.  
For example, If your Python version is 3.6, then python3.7m and 3.7.6_1 should be replaced with python3.6m and 3.6.  

```
PYTHON_LIBRARIES := boost_python3 **python3.7m**
PYTHON_INCLUDE := /usr/local/Cellar/python/**3.7.6_1**/Frameworks/Python.framework/Versions/3.7/include/python3.7m 
			/usr/local/lib/python3.7/site-packages/numpy/core/include/numpy
```

10. Modify FindvecLib.cmake file.
```
 open ~/caffe/cmake/Modules/FindvecLib.cmake
```
Change from 
```
 ${CMAKE_XCODE_DEVELOPER_DIR} to NO_DEFAULT_PATH)
```
with
```
 /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.15.sdk/System/Library/Frameworks/Accelerate.framework/Versions/Current/Frameworks/vecLib.framework/Headers/
```
**Note**  
For those who use future version of macOS, i.e. higher than 10.15, you need to change **MacOSX10.15.sdk** part with your version. For example, MacOSX10.16.sdk for 10.16 version.  

10. Pre-make
```
 cd ~/caffe
 mkdir build
 cd build
 cmake ..
```

11. Modify CMakeCache.txt file.  
```
 open ~/caffe/build/CMakeCache.txt
```
Edit as follows.  
**Note**  
Please change package versions accordingly with yours.  
You need to check both Python version and boost-python3 version.
```
//Path to a program.
PYTHON_EXECUTABLE:FILEPATH=/usr/local/bin/python3

//Path to a file.
PYTHON_INCLUDE_DIR:PATH=/usr/local/Cellar/python/3.7.6_1/Frameworks/Python.framework/Versions/3.7/include/python3.7m

//Path to a library.
PYTHON_LIBRARY:FILEPATH=/usr/local/Cellar/python/3.7.6_1/Frameworks/Python.framework/Versions/3.7/lib/libpython3.7m.dylib

//Flags used by the CXX compiler during all build types.
CMAKE_CXX_FLAGS:STRING=-std=c++11
  
//Boost python library (release)
Boost_PYTHON_LIBRARY_RELEASE:FILEPATH=/usr/local/Cellar/boost-python3/1.72.0/lib/libboost_python37-mt.dylib
  
//Build Caffe without CUDA support
CPU_ONLY:BOOL=ON
```

12. Modify Caffeconfig.cmake
```
 open ~/caffe/build
```

Find
```
 set(Caffe_CPU_ONLY OFF)
```
and replace with
```
 set(Caffe_CPU_ONLY ON)
```

13. Make Caffe again.
```
 cd ~/caffe/build
 cmake ..
 sudo make all
 sudo make pycaffe
```

14. Export paths of Caffe module.
If your default shell is zsh,
```
 open ~/.zshrc
```

else if it's bash
```
 open ~/.bash_profile
```

and type at the end of the file
```
 export PYTHONPATH=~/caffe/python:$PYTHONPATH
 export PATH=~/caffe/build/tools:$PATH
```

Hope this works well! :)

