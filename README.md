# DesaySV-DL
System setup for deep learning on Ubuntu 16.04.3.


# Setup NVIDIA Tool Chain

## 1. Install NVIDIA graphics card dirver
## 2. Install CUDA toolkit
## 3. Install cuDNN
1. Install from tarball

2. Install from .deb
```shell
$ sudo dpkg -i libcudnn7_7.0.1.13-1+cuda8.0_amd64.deb
$ sudo dpkg -i libcudnn7-dev_7.0.1.13-1+cuda8.0_amd64.deb
$ sudo dpkg -i libcudnn7-doc_7.0.1.13-1+cuda8.0_amd64.deb 
```
# Setup Deep Learning Frameworks

## Install Caffe
## Install Tensorflow
```shell
$ sudo apt-get install libcupti-dev
```
This requires installing libcudnn7, libcudnn7-dev, and libcudnn7-doc in certain directories. The headers must be located at:

```shell
/usr/include/cudnn.h
```
and libraries in:
```shell
/usr/lib/x86_64-linux-gnu/libcudnn_static.a
/usr/lib/x86_64-linux-gnu/libcudnn.so.7
/usr/lib/x86_64-linux-gnu/libcudnn.so
/usr/lib/x86_64-linux-gnu/libcudnn.so.7.0.1
/usr/lib/x86_64-linux-gnu/libcudnn_static_v7.a
```
