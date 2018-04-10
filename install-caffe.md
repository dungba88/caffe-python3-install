# install caffe with python3 on ubuntu 17.04

This document is to help people struggling with installing Caffe on Python3 and Ubuntu 17.04

Install the dependencies first, as specified in the below section. Then follow the rest of the guide. If there are some errors while compiling/running, please check the [troubleshooting](#troubleshooting) section and make sure all dependencies are installed.

### install dependencies

```shell
# python3 modules (numpy, protobuf, skimage)
sudo pip3 install numpy
sudo apt-get install python3-skimage
sudo apt-get install python3-protobuf

# build essential
sudo apt-get install build-essential cmake git pkg-config

# gflags, glog, lmdb
sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev

# boost
sudo apt-get install libboost-all-dev

# hdf5
sudo apt-get install libhdf5-dev

# protobuf
sudo apt-get install protobuf-compiler libprotobuf-dev

# blas
sudo apt-get install libblas-dev libcblas-dev libatlas-base-dev libopenblas-dev

# leveldb
sudo apt-get install libleveldb-dev

# snappy
sudo apt-get install libsnappy-dev
```

### install opencv3

Guide is taken from http://www.pyimagesearch.com/2015/07/20/install-opencv-3-0-and-python-3-4-on-ubuntu/

1. Checkout and compile

```shell
git clone https://github.com/opencv/opencv
cd opencv
mkdir build
cd build
cmake ..
make
sudo make install
```

### install caffe (without GPU support)

Guide is taken from http://caffe.berkeleyvision.org/install_apt.html

1. Checkout

```shell
cd ~
git clone https://github.com/BVLC/caffe
cd caffe
cp Makefile.config.example Makefile.config
```

2. Modify Makefile.config

- Uncomment `CPU_ONLY := 1`
- Uncomment `WITH_PYTHON_LAYER := 1`
- Uncomment `OPENCV_VERSION := 3`
- Uncomment section with Python3

3. Compile and test

```shell
make all
make test
make runtest
```

4. Install

By default, Caffe will not be installed (no target 'install' provided). We can copy the .so file manually

```shell
sudo cp build/lib/libcaffe.so* /usr/lib
```

5. Possible errors:

- Check [troubleshooting](#troubleshooting) section, issue 1, 2, 3, 6

### install caffe (with GPU support)

1. First install CUDA and (optional) CUDNN

```shell
sudo apt-get install nvidia-cuda-dev nvidia-cuda-toolkit nvidia-nsight
```

It will also automatically install the latest packaged version of NVIDIA, which is 375.66 as of Jul 2017, if you haven't installed it already.

For CUDNN, you need to register as a developer in https://developer.nvidia.com/cudnn. Then you will be able to download CUDNN. Download the runtime and developer library and install in that order.

2. Install everything else

The rest are mostly the same with the non-GPU setup, but there are some differences:
- In Makefile.config, uncomment USE_CDNN (if you want to use CuDNN, though that it's not required) and comment CPU_ONLY
- In Makefile.config, since I installed CUDA via apt-get, so CUDA_DIR := /usr needs to be uncommented
- In Makefile, since I installed on Ubuntu 17.04, which has GCC6 that CUDA doesn't support, so CXX need to be set to clang-3.8. Find NVCCFLAGS += -ccbin=$(CXX) and change $(CXX) to clang-3.8. I couldn't set CUSTOM_CXX in Makefile.config to clang-3.8 because another error comes up (`Cannot static link with compiler clang-3.8`)

2. Possible errors:

- Check [troubleshooting](#troubleshooting) section, issue 1, 2, 3, 5, 6

### install pycaffe

1. Compile

```shell
make pycaffe
```
2. Install

Install pycaffe manually by copying to dist-packages (in Ubuntu 17.04 it's in /usr/local/lib, but make sure you verify the path first)

```shell
sudo cp -r python/caffe/ /usr/local/lib/python3.5/dist-packages/
```

3. Test

Make sure the below command works without any errors

```shell
python3 -c "import caffe"
```

4. Possible errors:

- Check [troubleshooting](#troubleshooting) section, issue 4

### troubleshooting

1. hdf5.h: No such file or directory

In Makefile.config
- Add `/usr/include/hdf5/serial` to INCLUDE_DIRS (https://github.com/BVLC/caffe/issues/4808)

In Makefile
- Change `hdf5_hl` and `hdf5` in LIBRARIES to `hdf5_serial_hl` and `hdf5_serial` respectively

2. Undefined symbol cblas_*

In Makefile.config
- Change `BLAS := atlas` to `BLAS := open` to use openblas instead

3. ld cannot find lboost_python3

- Create symbolic link from libboost_python-py35.so to libboost_python3 (https://github.com/BVLC/caffe/issues/4843)

4. numpy/arrayobject.h not found

Make sure you get the correct path of Python dist-packages directory. In Ubuntu 17.04 it's in /usr/local/lib. I remember earlier version it's in /usr/lib.
In Makefile.config
- Find PYTHON_INCLUDE, change `/usr/lib/python3.5/dist-packages/numpy/core/include` to `/usr/local/lib/python3.5/dist-packages/numpy/core/include`

5. libcaffe.so undefined reference to caffe::*

- Make sure that you have `make clean` before `make all`.
- Also delete the libcaffe.so* we have copied to /usr/lib
```shell
sudo rm -f /usr/lib/libcaffe.so*
```

6. SyntaxError: invalid syntax in rrule.py

```shell
pip3 install python-dateutil --upgrade 
```

(https://github.com/dungba88/caffe-python3-install/issues/1)
