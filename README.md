# DesaySV-DL
System setup for deep learning on Ubuntu 16.04.3 with single NVIDIA Geforce GTX 1080.


# Setup NVIDIA Tool Chain

## 1. Install NVIDIA graphics card driver

Installing NVIDIA graphics driver is usually a headache! However you could just skip it! (not always)

Actually you don't need to install a NVIDIA graphics driver as CUDA Toolkit comes with pre-packaged GPU drivers. The NVIDIA graphics card driver will be installed together with CUDA as a by-product. However you need to install from the CUDA `.run` file and choose whether or not you want to install driver as well. 

However, you need to make sure that the CUDA driver bundled with CUDA toolkit does support your graphics card. It is still recommended to install the latest standalone driver separately as the driver bundled with CUDA is usually out-of-date.

There are three ways to install NVIDIA proprietary drivers.

1. Download from official NVIDIA website and follow their installation instructions. (risky and may need manual tweeking)
2. Using a PPA repository [Install Nvidia Drivers from PPA](https://askubuntu.com/questions/851069/latest-nvidia-driver-on-ubuntu-16-04)
3. (**Recommended**) Using `Additional Drivers` under `Software & Updates`. In your Ubuntu press the `Win` key and search `Additional Driver` and select for the recommended NVIDIA proprietary driver from official Ubuntu package repository.

When the driver is loaded, the driver version can be found by executing the command:
```shell
$ cat /proc/driver/nvidia/version
```
Run the NVIDIA system manager interface tool to query the status of all your GPUs:

```shell
$ nvidia-smi
```
You can check the status of driver modules loaded in the Linux Kernel:

```shell
$ lsmod | grep nvidia  # there should be output
$ lsmod | grep nouveau  # there should be no output
```

## 2. Install CUDA toolkit
Go to [NVIDIA website](https://developer.nvidia.com/cuda-downloads) and download the CUDA Debian Installer (`.deb`).

```shell
$ sudo dpkg -i cuda-repo-ubuntu1604-8-0-local-ga2_8.0.61-1_amd64.deb
$ sudo apt-get update
$ sudo apt-get install cuda
```
In order to automatically set all the environment variables every time you open your terminal, put the following exports into your `~/.bashrc` file.

```shell
export CUDA_HOME=/usr/local/cuda-8.0
export PATH=$CUDA_HOME/bin:$PATH
export LD_LIBRARY_PATH=$CUDA_HOME/lib64:$LD_LIBRARY_PATH
```
In order for the above to take effect, `source` your `~/.bashrc` or re-open your terminal.
```shell
$ source ~/.bashrc
```

The version of the CUDA Toolkit can be checked by running:
```shell
$ nvcc -V
```
Verify CUDA installation by running the examples.
```shell
$ cuda-install-samples-8.0.sh ~
$ cd NVIDIA_CUDA-8.0_Samples/
$ make
```
NOTE: If compiler error cannot find `-lnvcuvid`, or skipped incompatible `libnvcuvid.so` need to change the settings to be consistent with the nvidia driver you are using (e.g. nvidia-375). A [lazy fix](https://devtalk.nvidia.com/default/topic/769578/cuda-setup-and-installation/cuda-6-5-cannot-find-lnvcuvid/2) would be to run:
```shell
find . -type f -execdir sed -i 's/UBUNTU_PKG_NAME = "nvidia-367"/UBUNTU_PKG_NAME = "nvidia-375"/g' '{}' \;
```
Run the examples:

```shell
$ cd bin/x86_64/linux/release/
$ ./deviceQuery
```

At the end of printout, you will see:
```shell
Result = PASS
```

## 3. Install cuDNN
Download respective packages from [NVIDIA website](https://developer.nvidia.com/rdp/form/cudnn-download-survey) as below and follow the corresponding instructions. You need to sign up an account with NVIDIA to download the files.

1. Install from tarball (**recommended**)

```shell
$ tar -xzvf cudnn-8.0-linux-x64-v7.tgz
```
Copy the following files into the CUDA Toolkit directory.
```shell
$ sudo cp -P cuda/include/cudnn.h /usr/local/cuda/include
$ sudo cp -P cuda/lib64/libcudnn* /usr/local/cuda/lib64
$ sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
$ sudo ldconfig
```
Adding `-P` retains the symbolic links, and avoids the error: `/sbin/ldconfig.real: /usr/local/cuda/lib64/libcudnn.so.7 is not a symbolic link`.

NOTE: If you install cuDNN from tarball, you need to add the destination directores (e.g. `/usr/local/cuda/lib64`) into `$LD_LIBRARY_PATH` in order for other packages to look for the libraries. Doing so by adding the following line into your `~/.bashrc`.

```shell
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
```

2. Install from .deb

cuDNN v7.0
```shell
$ sudo dpkg -i libcudnn7_7.0.1.13-1+cuda8.0_amd64.deb
$ sudo dpkg -i libcudnn7-dev_7.0.1.13-1+cuda8.0_amd64.deb
$ sudo dpkg -i libcudnn7-doc_7.0.1.13-1+cuda8.0_amd64.deb 
```
cuDNN v6.0
```shell
$ sudo dpkg -i libcudnn6_6.0.21-1+cuda8.0_amd64.deb
$ sudo dpkg -i libcudnn6-dev_6.0.21-1+cuda8.0_amd64.deb
$ sudo dpkg -i libcudnn6-doc_6.0.21-1+cuda8.0_amd64.deb
```
3. Verify cuDNN

To verify that cuDNN is installed and is running properly, compile the mnistCUDNN sample located in the `/usr/src/cudnn_samples_v7` directory in the debian file. (**Note: The samples code is installed with cuDNN deb file**)
Copy the cuDNN sample to a writable path:
```shell
$ cp -r /usr/src/cudnn_samples_v7/ $HOME
$ cd  $HOME/cudnn_samples_v7/mnistCUDNN
$ make clean && make
$ ./mnistCUDNN
```
If cuDNN is properly installed and running on your Linux system, you will see a message similar to the following:
```shell
Test passed!
```

# Setup Deep Learning Frameworks

## Install Caffe

1. General dependencies
```shell
$ sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
$ sudo apt-get install --no-install-recommends libboost-all-dev
$ sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev
```
2. CUDA

Refer to [Install CUDA toolkit](#2-install-cuda-toolkit). Note: CUDA 8 is required on Ubuntu 16.04.

3. BLAS

Choose one of the followings
```shell
$ sudo apt-get install libatlas-base-dev
```
or
```shell
$ sudo apt-get install libopenblas-dev
```

4. Python

Install `pip`, Python development package, and `numpy`
```shell
$ sudo apt-get install python-pip python-dev python-numpy
```
and install the following dependency packages to avoid `ImportError: No module named skimage.io` and `google.protobuf.internal`
```shell
$ pip install scikit-image
$ pip install protobuf
```

5. Complile Caffe using CMake (Recommended)

If you don't have `git` and `CMake` in your system, install them first
```shell
$ sudo apt install git cmake
```
Download Caffe and compile
```shell
$ git clone https://github.com/BVLC/caffe.git
$ cd caffe
$ mkdir build && cd build
$ cmake -DCUDA_USE_STATIC_CUDA_RUNTIME=OFF ..
$ make all -j8
$ make runtest -j8
```
NOTE: `CUDA_USE_STATIC_CUDA_RUNTIME` (Default `ON`)
-- When enabled the static version of the CUDA runtime library will be used
   in `CUDA_LIBRARIES`.
On 16.04, `aarch64` has issues with a static cuda runtime. So we need to disable `CUDA_USE_STATIC_CUDA_RUNTIME`.

In order for python to find `caffe` module, you need to set the env variable `PYTHONPATH` by adding the following line into your `~/.bashrc`. Otherwise, `ImportError: No module named caffe`. 

```shell
export PYTHONPATH=$HOME/caffe/python:$PYTHONPATH
```

5. Compile Caffe using Makefile.config (make)

If you are using `Makefile.config` and `make`, you need to add the hdf5 include directory
```shell
$ cp Makefile.config.example Makefile.config
$ echo "INCLUDE_DIRS += /usr/include/hdf5/serial/" >> Makefile.config
$ echo "LIBRARY_DIRS += /usr/lib/x86_64-linux-gnu/hdf5/serial/" >> Makefile.config
```
and uncomment the `USE_CUDNN := 1` to build with cuDNN acceleration, choose which `BLAS` you are using either `atlas` or `openblas`, etc.

```shell
$ make all -j8
$ make test -j8
# (Optional)
$ make runtest -j8
```

6. Test AlexNet

In `/build`, run
```shell
$ tools/caffe time --model=../models/bvlc_alexnet/deploy.prototxt --gpu=0
```

## Install TensorFlow
Please refer to the complete and up-to-date install instructions from [TensorFlow website](https://www.tensorflow.org/install/install_linux).

1. Install dependencies

```shell
$ sudo apt-get install libcupti-dev
```
This requires installing `libcudnn7`, `libcudnn7-dev`, and `libcudnn7-doc` in certain directories. The headers must be located at:

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
```shell
/usr/lib/x86_64-linux-gnu/libcudnn.so.6
/usr/lib/x86_64-linux-gnu/libcudnn_static_v6.a
/usr/lib/x86_64-linux-gnu/libcudnn.so.6.0.21
/usr/lib/x86_64-linux-gnu/libcudnn_static.a
```
Refer to section [Install cuDNN](#3-install-cudnn) and Install from .deb will setup everything.

2. Install TensorFlow via virtualenv for Python 2.7

Install and activate virtualenv:

```shell
$ sudo apt-get install python-pip python-dev python-virtualenv 
$ virtualenv --system-site-packages ~/tensorflow
```
Activate TensorFlow virtualenv environment: 
```shell
$ source ~/tensorflow/bin/activate
```
NOTE: Note that you must activate the virtualenv environment each time you use TensorFlow. Using the following command to deactivate TensorFlow environment when you are done with it.
```shell
(tensorflow)$ deactivate 
```

Install TensorFlow with GPU support in the virtual environment: 

```shell
(tensorflow)$ pip install --upgrade tensorflow-gpu
```
3. Verify the installation
Activate the virtualenv and run the following python code:
```shell
(tensorflow)$ python
(tensorflow)# Python
import tensorflow as tf
hello = tf.constant('Hello, TensorFlow!')
sess = tf.Session()
print(sess.run(hello))
```
The code prints "Hello, TensorFlow!"

4. Uninstall TensorFlow

```shell
$ rm -r ~/tensorflow
```
5. Common errors
 
```shell
ImportError: libcudnn.Version: cannot open shared object file:
  No such file or directory
```

Install the exact **Version** of libcudnn. TensorFlow may not support the latest versions of cuDNN. 
