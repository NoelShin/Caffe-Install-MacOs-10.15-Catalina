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
 
3. Type following command in Terminal
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
 
9. Modify Makefile.config
```
 cd ~/caffe
 open .
```

Open Makefile.config with texteditor.
Copy all lines in Makefile.config file in this repository to your Makefile.config file.

*Note*
If your Python version is different with 3.7.6_1, you need to revise these two lines in Makefile.config.
Change the bold parts accordingly.
For example, If your Python version is 3.6, then python3.7m and 3.7.6_1 should be replaced with python3.6m and 3.6.

```
PYTHON_LIBRARIES := boost_python3 *python3.7m*
PYTHON_INCLUDE := /usr/local/Cellar/python/*3.7.6_1*/Frameworks/Python.framework/Versions/3.7/include/python3.7m 
			/usr/local/lib/python3.7/site-packages/numpy/core/include/numpy
```
      

 






