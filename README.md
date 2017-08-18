# DesaySV-DL
System setup for deep learning on Ubuntu 16.04.3.


# Setup NVIDIA Tool Chain

## 1. Install NVIDIA graphics card dirver
## 2. Install CUDA toolkit

```shell
$ sudo dpkg -i cuda-repo-ubuntu1604-8-0-local-ga2_8.0.61-1_amd64.deb
$ sudo apt-get update
$ sudo apt-get install cuda
```
Put the follow into your `~/.bashrc`.

```shell
export CUDA_HOME=/usr/local/cuda-8.0
export PATH=$CUDA_HOME/bin:$PATH
export LD_LIBRARY_PATH=$CUDA_HOME/lib64:$LD_LIBRARY_PATH
```

## 3. Install cuDNN
1. Install from tarball

```shell
$ tar -xzvf cudnn-8.0-linux-x64-v7.tgz
```
Copy the following files into the CUDA Toolkit directory.
```shell
$ sudo cp -P cuda/include/cudnn.h /usr/local/cuda/include
$ sudo cp -P cuda/lib64/libcudnn* /usr/local/cuda/lib64
$ sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
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
3. Verify cnDNN

To verify that cuDNN is installed and is running properly, compile the mnistCUDNN sample located in the `/usr/src/cudnn_samples_v7` directory in the debian file.
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
