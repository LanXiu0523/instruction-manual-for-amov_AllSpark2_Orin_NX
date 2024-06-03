# amov

ssh

```
ssh amov@192.168.68.102
```

## Flash

```
# refer: https://docs.amovlab.com/allspark2-orin/#/src/开发帮助/环境部署/环境部署

mkdir  ~/orin_nx 
cd  ~/orin_nx 
# 下载orin_nx_flash.tar.gz
tar -xvf orin_nx_flash.tar.gz
cd orin_nx 
chmod +x script.sh
chmod +x amov_start.sh
	... 刷机用 暂略
```



## info

```
# 驱动版本
amov@tegra-ubuntu:~/lpq/installspace$ head -n 1 /etc/nv_tegra_release
# R35 (release), REVISION: 2.1, GCID: 32413640, BOARD: t186ref, EABI: aarch64, DATE: Tue Jan 24 23:38:33 UTC 2023
# 注意到 L4T-V:35.2.1 对应 Jetpack-V:5.1
# refer: https://jetsonhacks.com/jetpack-and-jetson-linux-l4t-versions/

# 内核版本
amov@tegra-ubuntu:~/lpq/installspace$ uname -a 
Linux tegra-ubuntu 5.10.104-tegra #58 SMP PREEMPT Tue Oct 17 11:51:25 CST 2023 aarch64 aarch64 aarch64 GNU/Linux

# 操作系统
amov@tegra-ubuntu:~/lpq/installspace$ cat /etc/lsb-release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=20.04
DISTRIB_CODENAME=focal
DISTRIB_DESCRIPTION="Ubuntu 20.04.5 LTS"

# cmake
amov@tegra-ubuntu:~/lpq/installspace$ cmake -version
cmake version 3.16.3
CMake suite maintained and supported by Kitware (kitware.com/cmake).
```



## install

### 换源 & ros

```
wget http://fishros.com/install -O fishros && . fishros 
1 1 2 1 1
```

### cuda

```
sudo apt-get install cuda-11.4

sudo vim ~/.bashrc

# add:
export PATH=/usr/local/cuda/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
export CUDA_HOME=$CUDA_HOME:/usr/local/cuda
# :add END

source ~/.bashrc

# check cuda版本
nvcc -V
```

### cudnn

```
sudo apt-get install libcudnn8

# check cuDNN版本
dpkg -l libcudnn8
```

### opencv

```
mkdir -p ~/lpq/installspace/
cd ~/lpq/installspace/
wget https://download.amovlab.com/model/install/common/opencv470-install.sh
# 若wget失败就用主机点开链接下载 然后scp到nx

chmod +x opencv470-install.sh
./opencv470-install.sh

# check opencv版本
dpkg -l libopencv / pkg-config opencv --modversion
python3
>>> import cv2
>>> cv2.__version__
```

### tensorrt

```
cd ~/lpq/installspace/
# refer link: https://developer.nvidia.com/nvidia-tensorrt-8x-download
# download link: https://developer.nvidia.com/compute/machine-learning/tensorrt/secure/8.2.5.1/local_repos/nv-tensorrt-repo-ubuntu2004-cuda11.4-trt8.2.5.1-ga-20220505_1-1_arm64.deb

sudo dpkg -i nv-tensorrt-repo-ubuntu2004-cuda11.4-trt8.2.5.1-ga-20220505_1-1_arm64.deb
sudo apt-get install tensorrt

# check tensorrt版本
dpkg -l tensorrt
```

### pytorch

```
cd ~/lpq/installspace/
# refer link: https://forums.developer.nvidia.com/t/pytorch-for-jetson/72048
# download link: https://developer.download.nvidia.com/compute/redist/jp/v51/pytorch/torch-1.14.0a0+44dac51c.nv23.02-cp38-cp38-linux_aarch64.whl?_gl=1*184oh1q*_gcl_au*MTAyMzg0NjM3NS4xNzE3NDIxODU5

sudo apt-get install python3-pip python3-dev
python3 -m pip install --upgrade pip
# pip换源
pip3 config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple

sudo apt-get install python3-pip libopenblas-base libopenmpi-dev libomp-dev
python3 -m pip install 'Cython<3'
python3 -m pip install numpy torch-1.14.0a0+44dac51c.nv23.02-cp38-cp38-linux_aarch64.whl


# check torch版本
python3
>>> import torch
>>> torch.__version__
```

### torchvision

```
# torchvision
sudo apt-get install libjpeg-dev zlib1g-dev libpython3-dev libopenblas-dev libavcodec-dev libavformat-dev libswscale-dev

# 根据 refer: https://forums.developer.nvidia.com/t/pytorch-for-jetson/72048
# 知 torch-V:1.14 对应 torchvision-V:0.14.1
git clone -b v0.14.1 https://github.com/pytorch/vision torchvision
cd torchvision

export BUILD_VERSION=0.14.0
export CUDA_HOME=/usr/local/cuda
python3 setup.py install --user 

cd ../  # attempting to load torchvision from build dir will result in import error
python3 -m pip install 'pillow<7' # always needed for Python 2.7, not needed torchvision v0.5.0+ with Python 3.6

# check torch & torchvision 是否安装成功并可调用gpu
python3
>>> import torch
>>> import torchvision
>>> torch.__version__
>>> torchvision.__version__
>>> torch.cuda.is_available() 
```

### referance

```
https://docs.amovlab.com/allspark2-orin/#/src/开发帮助/安装cuda/安装cuda

https://blog.csdn.net/liuyang_xyz/article/details/137174052?spm=1001.2014.3001.5502

https://blog.csdn.net/liuyang_xyz/article/details/137175752?spm=1001.2014.3001.5502

https://developer.nvidia.com/nvidia-tensorrt-8x-download

https://forums.developer.nvidia.com/t/pytorch-for-jetson/72048
```



## demo

```
mkdir ~/lpq/workspace/demo/

```

