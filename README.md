# Install Caffe with Python3 on Ubuntu 17.04

This document is to help people struggling with installing Caffe on Python3 and Ubuntu 17.04

### Install OpenCV3

Guide is taken from http://www.pyimagesearch.com/2015/07/20/install-opencv-3-0-and-python-3-4-on-ubuntu/

1. Install dependencies

```shell
sudo pip3 install numpy
sudo apt-get install build-essential cmake git pkg-config
 ```

2. Checkout and compile

```shell
git clone https://github.com/opencv/opencv
cd opencv
mkdir build
cd build
cmake ..
make
sudo make install
```

### Install Caffe (without GPU support)

Guide is taken from http://caffe.berkeleyvision.org/install_apt.html

1. Install dependencies

```shell
sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev
sudo apt-get install libboost-all-dev
sudo apt-get install libhdf5-dev
sudo apt-get install protobuf-compiler libprotobuf-dev
sudo apt-get install libblas-dev libcblas-dev libatlas-base-dev libopenblas-dev
sudo apt-get install libleveldb-dev
sudo apt-get install libsnappy-dev
```

2. Checkout

```shell
cd ~
git clone https://github.com/BVLC/caffe
cd caffe
cp Makefile.config.example Makefile.config
```

3. Modify Makefile.config

- Uncomment `CPU_ONLY := 1`
- Uncomment `WITH_PYTHON_LAYER := 1`
- Uncomment section with Python3
- Uncomment `OPENCV_VERSION := 3`
- Add `/usr/include/hdf5/serial` to INCLUDE_DIRS (https://github.com/BVLC/caffe/issues/4808)
- Change `BLAS := atlas` to `BLAS := open` to use openblas instead. I have `undefined symbol cblas_sgemm` when using atlas.

4. Modify Makefile

- Change `hdf5_hl` and `hdf5` in LIBRARIES to `hdf5_serial_hl` and `hdf5_serial` respectively

5. Additional changes

- Create symbolic link from libboost_python-py35.so to libboost_python3 (https://github.com/BVLC/caffe/issues/4843)

6. Compile and test

```shell
make all
make test
make runtest
```

### Troubleshooting

1. hdf5.h: No such file or directory

- Add `/usr/include/hdf5/serial` to INCLUDE_DIRS (https://github.com/BVLC/caffe/issues/4808)
- Change `hdf5_hl` and `hdf5` in LIBRARIES to `hdf5_serial_hl` and `hdf5_serial` respectively

2. Undefined symbol cblas_*

- Change `BLAS := atlas` to `BLAS := open` to use openblas instead

3. ld cannot find lboost_python3

- Create symbolic link from libboost_python-py35.so to libboost_python3 (https://github.com/BVLC/caffe/issues/4843)
