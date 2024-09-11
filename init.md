# amov

ssh

```
ssh amov@192.168.122.102
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

# 操作系统版本
amov@tegra-ubuntu:~/lpq/installspace$ cat /etc/lsb-release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=20.04
DISTRIB_CODENAME=focal
DISTRIB_DESCRIPTION="Ubuntu 20.04.5 LTS"

# cmake版本
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

### new torch & torchvision & torchaudio install
```
refer: https://forums.developer.nvidia.com/t/pytorch-for-jetson/72048
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

### jtop

```
# Jetson-stats 可以查看显卡状态 类似于win的nvidia-smi
sudo -H pip3 install -U jetson-stats

# 安装好后需要重启
sudo reboot

# 使用jtop
jtop
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
mkdir ~/lpq/workspace/
cd ~/lpq/workspace/
git clone https://github.com/LanXiu0523/amov_nx.git
cd amov_nx/demo/mnist/

python3 -m pip install -r requirements.txt
python3 main.py
```

output logs:

```
amov@tegra-ubuntu:~/lpq/workspace/amov_nx/demo/mnist$ python3 main.py

Downloading https://ossci-datasets.s3.amazonaws.com/mnist/train-images-idx3-ubyte.gz
Downloading https://ossci-datasets.s3.amazonaws.com/mnist/train-images-idx3-ubyte.gz to ../data/MNIST/raw/train-images-idx3-ubyte.gz
100.0%
Extracting ../data/MNIST/raw/train-images-idx3-ubyte.gz to ../data/MNIST/raw

Downloading https://ossci-datasets.s3.amazonaws.com/mnist/train-labels-idx1-ubyte.gz
Downloading https://ossci-datasets.s3.amazonaws.com/mnist/train-labels-idx1-ubyte.gz to ../data/MNIST/raw/train-labels-idx1-ubyte.gz
100.0%
Extracting ../data/MNIST/raw/train-labels-idx1-ubyte.gz to ../data/MNIST/raw

Downloading https://ossci-datasets.s3.amazonaws.com/mnist/t10k-images-idx3-ubyte.gz
Downloading https://ossci-datasets.s3.amazonaws.com/mnist/t10k-images-idx3-ubyte.gz to ../data/MNIST/raw/t10k-images-idx3-ubyte.gz
100.0%
Extracting ../data/MNIST/raw/t10k-images-idx3-ubyte.gz to ../data/MNIST/raw

Downloading https://ossci-datasets.s3.amazonaws.com/mnist/t10k-labels-idx1-ubyte.gz
Downloading https://ossci-datasets.s3.amazonaws.com/mnist/t10k-labels-idx1-ubyte.gz to ../data/MNIST/raw/t10k-labels-idx1-ubyte.gz
100.0%
Extracting ../data/MNIST/raw/t10k-labels-idx1-ubyte.gz to ../data/MNIST/raw

Train Epoch: 1 [0/60000 (0%)]	Loss: 2.302567
Train Epoch: 1 [640/60000 (1%)]	Loss: 1.517678
Train Epoch: 1 [1280/60000 (2%)]	Loss: 0.687616
Train Epoch: 1 [1920/60000 (3%)]	Loss: 0.667161
Train Epoch: 1 [2560/60000 (4%)]	Loss: 0.451334
Train Epoch: 1 [3200/60000 (5%)]	Loss: 0.382143
Train Epoch: 1 [3840/60000 (6%)]	Loss: 0.193430
Train Epoch: 1 [4480/60000 (7%)]	Loss: 0.460241
Train Epoch: 1 [5120/60000 (9%)]	Loss: 0.268638
Train Epoch: 1 [5760/60000 (10%)]	Loss: 0.125308
Train Epoch: 1 [6400/60000 (11%)]	Loss: 0.234459
Train Epoch: 1 [7040/60000 (12%)]	Loss: 0.375423
Train Epoch: 1 [7680/60000 (13%)]	Loss: 0.274937
Train Epoch: 1 [8320/60000 (14%)]	Loss: 0.156508
Train Epoch: 1 [8960/60000 (15%)]	Loss: 0.322475
Train Epoch: 1 [9600/60000 (16%)]	Loss: 0.116671
Train Epoch: 1 [10240/60000 (17%)]	Loss: 0.232286
Train Epoch: 1 [10880/60000 (18%)]	Loss: 0.323425
Train Epoch: 1 [11520/60000 (19%)]	Loss: 0.239488
Train Epoch: 1 [12160/60000 (20%)]	Loss: 0.128182
Train Epoch: 1 [12800/60000 (21%)]	Loss: 0.184412
Train Epoch: 1 [13440/60000 (22%)]	Loss: 0.134306
Train Epoch: 1 [14080/60000 (23%)]	Loss: 0.198096
Train Epoch: 1 [14720/60000 (25%)]	Loss: 0.216466
Train Epoch: 1 [15360/60000 (26%)]	Loss: 0.206289
Train Epoch: 1 [16000/60000 (27%)]	Loss: 0.419045
Train Epoch: 1 [16640/60000 (28%)]	Loss: 0.070359
Train Epoch: 1 [17280/60000 (29%)]	Loss: 0.122586
Train Epoch: 1 [17920/60000 (30%)]	Loss: 0.077080
Train Epoch: 1 [18560/60000 (31%)]	Loss: 0.267868
Train Epoch: 1 [19200/60000 (32%)]	Loss: 0.108636
Train Epoch: 1 [19840/60000 (33%)]	Loss: 0.093852
Train Epoch: 1 [20480/60000 (34%)]	Loss: 0.199320
Train Epoch: 1 [21120/60000 (35%)]	Loss: 0.153249
Train Epoch: 1 [21760/60000 (36%)]	Loss: 0.304516
Train Epoch: 1 [22400/60000 (37%)]	Loss: 0.132193
Train Epoch: 1 [23040/60000 (38%)]	Loss: 0.189189
Train Epoch: 1 [23680/60000 (39%)]	Loss: 0.190133
Train Epoch: 1 [24320/60000 (41%)]	Loss: 0.045417
Train Epoch: 1 [24960/60000 (42%)]	Loss: 0.068733
Train Epoch: 1 [25600/60000 (43%)]	Loss: 0.094305
Train Epoch: 1 [26240/60000 (44%)]	Loss: 0.257679
Train Epoch: 1 [26880/60000 (45%)]	Loss: 0.263702
Train Epoch: 1 [27520/60000 (46%)]	Loss: 0.032389
Train Epoch: 1 [28160/60000 (47%)]	Loss: 0.087677
Train Epoch: 1 [28800/60000 (48%)]	Loss: 0.044122
Train Epoch: 1 [29440/60000 (49%)]	Loss: 0.053606
Train Epoch: 1 [30080/60000 (50%)]	Loss: 0.161719
Train Epoch: 1 [30720/60000 (51%)]	Loss: 0.151700
Train Epoch: 1 [31360/60000 (52%)]	Loss: 0.206779
Train Epoch: 1 [32000/60000 (53%)]	Loss: 0.165718
Train Epoch: 1 [32640/60000 (54%)]	Loss: 0.091473
Train Epoch: 1 [33280/60000 (55%)]	Loss: 0.038802
Train Epoch: 1 [33920/60000 (57%)]	Loss: 0.136976
Train Epoch: 1 [34560/60000 (58%)]	Loss: 0.070308
Train Epoch: 1 [35200/60000 (59%)]	Loss: 0.126329
Train Epoch: 1 [35840/60000 (60%)]	Loss: 0.142839
Train Epoch: 1 [36480/60000 (61%)]	Loss: 0.202180
Train Epoch: 1 [37120/60000 (62%)]	Loss: 0.123089
Train Epoch: 1 [37760/60000 (63%)]	Loss: 0.090552
Train Epoch: 1 [38400/60000 (64%)]	Loss: 0.107376
Train Epoch: 1 [39040/60000 (65%)]	Loss: 0.051508
Train Epoch: 1 [39680/60000 (66%)]	Loss: 0.440532
Train Epoch: 1 [40320/60000 (67%)]	Loss: 0.158497
Train Epoch: 1 [40960/60000 (68%)]	Loss: 0.076022
Train Epoch: 1 [41600/60000 (69%)]	Loss: 0.364558
Train Epoch: 1 [42240/60000 (70%)]	Loss: 0.148041
Train Epoch: 1 [42880/60000 (71%)]	Loss: 0.043070
Train Epoch: 1 [43520/60000 (72%)]	Loss: 0.033125
Train Epoch: 1 [44160/60000 (74%)]	Loss: 0.130088
Train Epoch: 1 [44800/60000 (75%)]	Loss: 0.057304
Train Epoch: 1 [45440/60000 (76%)]	Loss: 0.239740
Train Epoch: 1 [46080/60000 (77%)]	Loss: 0.044308
Train Epoch: 1 [46720/60000 (78%)]	Loss: 0.032266
Train Epoch: 1 [47360/60000 (79%)]	Loss: 0.105848
Train Epoch: 1 [48000/60000 (80%)]	Loss: 0.145656
Train Epoch: 1 [48640/60000 (81%)]	Loss: 0.146950
Train Epoch: 1 [49280/60000 (82%)]	Loss: 0.058196
Train Epoch: 1 [49920/60000 (83%)]	Loss: 0.023243
Train Epoch: 1 [50560/60000 (84%)]	Loss: 0.035635
Train Epoch: 1 [51200/60000 (85%)]	Loss: 0.064052
Train Epoch: 1 [51840/60000 (86%)]	Loss: 0.145339
Train Epoch: 1 [52480/60000 (87%)]	Loss: 0.091939
Train Epoch: 1 [53120/60000 (88%)]	Loss: 0.086138
Train Epoch: 1 [53760/60000 (90%)]	Loss: 0.103459
Train Epoch: 1 [54400/60000 (91%)]	Loss: 0.064102
Train Epoch: 1 [55040/60000 (92%)]	Loss: 0.119990
Train Epoch: 1 [55680/60000 (93%)]	Loss: 0.201028
Train Epoch: 1 [56320/60000 (94%)]	Loss: 0.225151
Train Epoch: 1 [56960/60000 (95%)]	Loss: 0.114258
Train Epoch: 1 [57600/60000 (96%)]	Loss: 0.116538
Train Epoch: 1 [58240/60000 (97%)]	Loss: 0.077795
Train Epoch: 1 [58880/60000 (98%)]	Loss: 0.077870
Train Epoch: 1 [59520/60000 (99%)]	Loss: 0.136184

Test set: Average loss: 0.0469, Accuracy: 9848/10000 (98%)

......


Train Epoch: 14 [0/60000 (0%)]	Loss: 0.003617
Train Epoch: 14 [640/60000 (1%)]	Loss: 0.026607
Train Epoch: 14 [1280/60000 (2%)]	Loss: 0.009354
Train Epoch: 14 [1920/60000 (3%)]	Loss: 0.043224
Train Epoch: 14 [2560/60000 (4%)]	Loss: 0.001962
Train Epoch: 14 [3200/60000 (5%)]	Loss: 0.020590
Train Epoch: 14 [3840/60000 (6%)]	Loss: 0.021738
Train Epoch: 14 [4480/60000 (7%)]	Loss: 0.005573
Train Epoch: 14 [5120/60000 (9%)]	Loss: 0.040054
Train Epoch: 14 [5760/60000 (10%)]	Loss: 0.087472
Train Epoch: 14 [6400/60000 (11%)]	Loss: 0.007701
Train Epoch: 14 [7040/60000 (12%)]	Loss: 0.021306
Train Epoch: 14 [7680/60000 (13%)]	Loss: 0.029504
Train Epoch: 14 [8320/60000 (14%)]	Loss: 0.001148
Train Epoch: 14 [8960/60000 (15%)]	Loss: 0.009970
Train Epoch: 14 [9600/60000 (16%)]	Loss: 0.004563
Train Epoch: 14 [10240/60000 (17%)]	Loss: 0.001536
Train Epoch: 14 [10880/60000 (18%)]	Loss: 0.008075
Train Epoch: 14 [11520/60000 (19%)]	Loss: 0.006924
Train Epoch: 14 [12160/60000 (20%)]	Loss: 0.001368
Train Epoch: 14 [12800/60000 (21%)]	Loss: 0.009053
Train Epoch: 14 [13440/60000 (22%)]	Loss: 0.011353
Train Epoch: 14 [14080/60000 (23%)]	Loss: 0.012305
Train Epoch: 14 [14720/60000 (25%)]	Loss: 0.007575
Train Epoch: 14 [15360/60000 (26%)]	Loss: 0.002559
Train Epoch: 14 [16000/60000 (27%)]	Loss: 0.000938
Train Epoch: 14 [16640/60000 (28%)]	Loss: 0.034927
Train Epoch: 14 [17280/60000 (29%)]	Loss: 0.002078
Train Epoch: 14 [17920/60000 (30%)]	Loss: 0.000166
Train Epoch: 14 [18560/60000 (31%)]	Loss: 0.001878
Train Epoch: 14 [19200/60000 (32%)]	Loss: 0.002671
Train Epoch: 14 [19840/60000 (33%)]	Loss: 0.032304
Train Epoch: 14 [20480/60000 (34%)]	Loss: 0.002712
Train Epoch: 14 [21120/60000 (35%)]	Loss: 0.003662
Train Epoch: 14 [21760/60000 (36%)]	Loss: 0.030319
Train Epoch: 14 [22400/60000 (37%)]	Loss: 0.008629
Train Epoch: 14 [23040/60000 (38%)]	Loss: 0.130774
Train Epoch: 14 [23680/60000 (39%)]	Loss: 0.007597
Train Epoch: 14 [24320/60000 (41%)]	Loss: 0.040109
Train Epoch: 14 [24960/60000 (42%)]	Loss: 0.010573
Train Epoch: 14 [25600/60000 (43%)]	Loss: 0.029216
Train Epoch: 14 [26240/60000 (44%)]	Loss: 0.019143
Train Epoch: 14 [26880/60000 (45%)]	Loss: 0.001368
Train Epoch: 14 [27520/60000 (46%)]	Loss: 0.009930
Train Epoch: 14 [28160/60000 (47%)]	Loss: 0.030858
Train Epoch: 14 [28800/60000 (48%)]	Loss: 0.003466
Train Epoch: 14 [29440/60000 (49%)]	Loss: 0.002524
Train Epoch: 14 [30080/60000 (50%)]	Loss: 0.001668
Train Epoch: 14 [30720/60000 (51%)]	Loss: 0.031526
Train Epoch: 14 [31360/60000 (52%)]	Loss: 0.002528
Train Epoch: 14 [32000/60000 (53%)]	Loss: 0.005050
Train Epoch: 14 [32640/60000 (54%)]	Loss: 0.031563
Train Epoch: 14 [33280/60000 (55%)]	Loss: 0.040528
Train Epoch: 14 [33920/60000 (57%)]	Loss: 0.005908
Train Epoch: 14 [34560/60000 (58%)]	Loss: 0.017556
Train Epoch: 14 [35200/60000 (59%)]	Loss: 0.013665
Train Epoch: 14 [35840/60000 (60%)]	Loss: 0.003115
Train Epoch: 14 [36480/60000 (61%)]	Loss: 0.066766
Train Epoch: 14 [37120/60000 (62%)]	Loss: 0.005755
Train Epoch: 14 [37760/60000 (63%)]	Loss: 0.004617
Train Epoch: 14 [38400/60000 (64%)]	Loss: 0.088156
Train Epoch: 14 [39040/60000 (65%)]	Loss: 0.046651
Train Epoch: 14 [39680/60000 (66%)]	Loss: 0.013924
Train Epoch: 14 [40320/60000 (67%)]	Loss: 0.010813
Train Epoch: 14 [40960/60000 (68%)]	Loss: 0.106454
Train Epoch: 14 [41600/60000 (69%)]	Loss: 0.007424
Train Epoch: 14 [42240/60000 (70%)]	Loss: 0.037713
Train Epoch: 14 [42880/60000 (71%)]	Loss: 0.009523
Train Epoch: 14 [43520/60000 (72%)]	Loss: 0.027602
Train Epoch: 14 [44160/60000 (74%)]	Loss: 0.013503
Train Epoch: 14 [44800/60000 (75%)]	Loss: 0.001743
Train Epoch: 14 [45440/60000 (76%)]	Loss: 0.008300
Train Epoch: 14 [46080/60000 (77%)]	Loss: 0.024538
Train Epoch: 14 [46720/60000 (78%)]	Loss: 0.006560
Train Epoch: 14 [47360/60000 (79%)]	Loss: 0.009326
Train Epoch: 14 [48000/60000 (80%)]	Loss: 0.017371
Train Epoch: 14 [48640/60000 (81%)]	Loss: 0.002416
Train Epoch: 14 [49280/60000 (82%)]	Loss: 0.003250
Train Epoch: 14 [49920/60000 (83%)]	Loss: 0.000716
Train Epoch: 14 [50560/60000 (84%)]	Loss: 0.000803
Train Epoch: 14 [51200/60000 (85%)]	Loss: 0.020629
Train Epoch: 14 [51840/60000 (86%)]	Loss: 0.001138
Train Epoch: 14 [52480/60000 (87%)]	Loss: 0.011617
Train Epoch: 14 [53120/60000 (88%)]	Loss: 0.007175
Train Epoch: 14 [53760/60000 (90%)]	Loss: 0.004735
Train Epoch: 14 [54400/60000 (91%)]	Loss: 0.006936
Train Epoch: 14 [55040/60000 (92%)]	Loss: 0.025248
Train Epoch: 14 [55680/60000 (93%)]	Loss: 0.008075
Train Epoch: 14 [56320/60000 (94%)]	Loss: 0.054928
Train Epoch: 14 [56960/60000 (95%)]	Loss: 0.004667
Train Epoch: 14 [57600/60000 (96%)]	Loss: 0.018515
Train Epoch: 14 [58240/60000 (97%)]	Loss: 0.031570
Train Epoch: 14 [58880/60000 (98%)]	Loss: 0.002801
Train Epoch: 14 [59520/60000 (99%)]	Loss: 0.002787

Test set: Average loss: 0.0252, Accuracy: 9916/10000 (99%)
```

