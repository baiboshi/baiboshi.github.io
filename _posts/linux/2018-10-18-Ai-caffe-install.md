---
layout: post
title: "安装部署caffe"
tags: [linux]
---
##安装环境准备
>1.系统需求

     O linux(centos)系统
>2.硬件环境需求

     O 安装有GPU显卡的服务器
>3.软件环境

     O  CUDA8.0显卡驱动
     O  caffe源码包
>2.安装部署CUDA8.0显卡驱动
>>1.下载CUDA8.0显卡驱动

~~~wget
wget https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda-repo-rhel7-8-0-local-ga2-8.0.61-1.x86_64-rpm
~~~

>>2.安装

~~~rpm
rpm -ivh cuda-repo-rhel7-8-0-local-ga2-8.0.61-1.x86_64-rpm
yum -y install cuda
~~~

>>3.测试是否安装成功

![](/images/linux/Ai-gpu-xianka.png)
>3.安装caffe
>>1.安装caffe所需软件依赖包

~~~yum
yum -y install protobuf-devel leveldb-devel snappy-devel opencv-devel boost-devel hdf5-devel git gflags-devel glog-devel lmdb-devel openblas-devel cmake
~~~

>>2.检查是否安装成功

~~~rpm
rpm -q protobuf-devel leveldb-devel snappy-devel opencv-devel boost-devel hdf5-devel git gflags-devel glog-devel lmdb-devel openblas-devel cmake
~~~
![](/images/linux/caffe.yilaibao.png)
>>3.安装python模块
>>>1.下载

~~~wget
https://zh.osdn.net/projects/sfnet_numpy/downloads/NumPy/1.7.1/numpy-1.7.1.tar.gz
~~~
>>>安装

~~~tar
tar xf numpy-1.7.1.tar.gz
cd numpy-1.7.1/
python setup.py install
~~~
>>4.安装caffe源码包
>>>1.下载

~~~git
git clone ne https://github.com/BVLC/caffe.git
~~~
>>>2.编译

~~~cd
cd caffe/ && make
~~~
>>>3.擦看编译是否成功

![](/images/linux/caffe.bianyi.png)
>>>4.执行caffe/examples/mnist/train_lenet.sh 脚本

~~~.
./examples/mnist/train_lenet.sh
~~~
![](/images/linux/caffe.jiaoben-1.png)
![](/images/linux/caffe.jiaoben-2.png)
>>>4.执行caffe/examples/mnist/ create_mnist.sh 脚本

~~~.
./examples/mnist/ create_mnist.sh
~~~
![](/images/linux/caffe.jiaoben-3.png)

