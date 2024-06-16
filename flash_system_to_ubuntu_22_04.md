# amov_flash_system_to_ubuntu_22_04

ssh

```
ssh lpq@192.168.0.199
ssh ubuntu@192.168.0.170
```

## prepare

- 准备一台 Ubuntu 64位 20.04 或 22.04，可以是虚拟机，剩余空间至少50G，我这里采用 Ubuntu 64位 20.04的虚拟机，称为：**Ubuntu主机**
- 准备一台 amov AllSpark2 Orin NX，称为：**NX**
- （可选）一台电脑，方便传文件，称为：**个人电脑**
- 上述机器需在同一个局域网内，**Ubuntu主机**和**Ubuntu主机**可以是路由器网线连接、Wi-Fi连接，**NX**必须是路由器网线连接

```
# network ip adress detils
# Ubuntu主机 20.04 ubuntu@192.168.0.170
# (可选)个人电脑 lanxiu@192.168.0.159
```

**Ubuntu主机上：**

```
sudo apt update
sudo apt install openssh-server
mkdir -p /home/ubuntu/lpq/installspace/flash
```

## download

**个人电脑上：**

```
# enter https://developer.nvidia.com/sdk-manager
# click download .deb Ubuntu 
# enter https://download.amovlab.com/orin_nx/amov-orin-jetson3630.deb
# 将 nvidia sdk-manager scp 到Ubuntu主机上
scp /Users/lanxiu/Downloads/sdkmanager_2.1.0-11682_amd64.deb ubuntu@192.168.0.170:/home/ubuntu/lpq/installspace/flash
```

## flash by nvidia sdk-manager

**Ubuntu主机上：**

### run sdk-manager

```
cd /home/ubuntu/lpq/installspace/flash/
sudo apt install ./sdkmanager_2.1.0-11682_amd64.deb

lsusb
# current logs:
xxx

# NX 下电，将一根 Micro-B 数据线（或OTG数据线）的 Micro-B 接口（或OTG接口）接在 NX 上，将另一端的 USB 3.0 接口接在 Ubuntu 主机上，NX 按下 REC 按键再接通电源，并保持 3 秒。

lsusb
# current logs:
xxx

# run sdk-manager (千万不要 sudo 运行！)
sdkmanager

# login ...
```

### STEP 01

```
# 没✅就是不选（尽量不要多选！）
  # PRODUCT CATEGORY: 
  	# Jetson ✅
  # SYSTEM CONFIGURATION: 
  	# Target Hardware: Jetson Orin NX 16GB ✅
  # SDK VERSION:
  	# JetPack 6.0 (rev.2) ✅
# click CONTINUE
```

### STEP 02

```
# 勾选我同意，勾选先下载到本地
  # I accept ... ✅
  # Download now. Install later. ✅
# click CONTINUE
# waiting ...

# （重要！）下载完成后，会跳转至 STEP 04，点击 BACK TO STEP 01（在 FINISH 下面一行灰色小字）
# STEP 01
	# PRODUCT CATEGORY: 
  	# Jetson ✅
  # SYSTEM CONFIGURATION: 
  	# Target Hardware: Jetson Orin NX 16GB ✅
  # SDK VERSION:
  	# JetPack 6.0 (rev.2) ✅
# click CONTINUE

# STEP 02
	# 仅勾选我同意
		# I accept ... ✅
# click CONTINUE
```

### STEP 03

```
# SDK Manager is about to flash your Jetson Nano module
	# Selected device: Jetson Orin NX 16GB
	# OEM Configuration: Pre-Config
		# New Username: [NX 的新用户名]
		# New Password: [NX 的新密码]
	# Storage Device: NVMe （必须选NVMe！）
# click Flash
# waiting ...

# SDK Manager is about to install sDK components on yourJetson Nano module
	# 此时将 NX 用 HDMI 线连接屏幕，连接有线网，查看 NX 的 ip 地址（确保与 Ubuntu主机 在同一个子网）
		# network ip adress detils
		# NX lpq@192.168.0.199
	# Connection: Ethernet
	# IPv4: 192.168.0.199
	# Username: lpq
	# Password: lpq
# click install
# waiting ...
```

### STEP 04

```
```









### new

### STEP 01

```
# 没✅就是不选！
  # PRODUCT CATEGORY: 
  	# Jetson ✅
  # SYSTEM CONFIGURATION: 
  	# Target Hardware: Jetson Orin NX 16GB ✅
  # SDK VERSION:
  	# JetPack 6.0 (rev.2) ✅
  # ADDITIONAL SDKS
  	# DeepStream ✅
# click CONTINUE
```

### STEP 02

```
# 勾选全部可安装模块，勾选我同意，勾选先下载到本地
	# 勾选全部可安装模块
  # I accept ... ✅
  # Download now. Install later. ✅
# click CONTINUE
# waiting ...

# （重要！）下载完成后，会跳转至 STEP 04，点击 BACK TO STEP 01（在 FINISH 下面一行灰色小字）
# STEP 01
	# PRODUCT CATEGORY: 
  	# Jetson ✅
  # SYSTEM CONFIGURATION: 
  	# Target Hardware: Jetson Orin NX 16GB ✅
  # SDK VERSION:
  	# JetPack 6.0 (rev.2) ✅
# click CONTINUE

# STEP 02
	# 仅勾选我同意
		# I accept ... ✅
# click CONTINUE
```

### STEP 03

```
# SDK Manager is about to flash your Jetson Nano module
	# Selected device: Jetson Orin NX 16GB
	# OEM Configuration: Pre-Config
		# New Username: [NX 的新用户名]
		# New Password: [NX 的新密码]
	# Storage Device: NVMe （必须选NVMe！）
# click Flash
# waiting ...

# SDK Manager is about to install sDK components on yourJetson Nano module
	# 此时将 NX 用 HDMI 线连接屏幕，连接有线网，查看 NX 的 ip 地址（确保与 Ubuntu主机 在同一个子网）
		# network ip adress detils
		# NX lpq@192.168.0.199
	# Connection: Ethernet
	# IPv4: 192.168.0.199
	# Username: lpq
	# Password: lpq
# click install
# waiting ...
```

### STEP 04

```

```







```


refer: 
https://docs.amovlab.com//allspark2-orin/#/src/开发帮助/升级/升级?id=使用sdkmanager-升级适配-ubuntu-2204-jetson-linux-3630（jetpack-60）
https://bbs.amovlab.com/forum.php?mod=viewthread&tid=44381&highlight=22
https://developer.nvidia.com/sdk-manager
https://github.com/LanXiu0523/YOLOv3-Realtime-Cam-Detection-on-Manifold2-NVIDIA-Jetson-TX2
```



