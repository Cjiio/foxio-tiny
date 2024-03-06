

简体中文 | [English](./README-en.md)

<br>

# 目录

- [目录](#目录)
- [项目简介](#项目简介)
  - [硬件资料](#硬件资料)
  - [芯片规格](#芯片规格)
- [SDK目录结构](#sdk目录结构)
- [快速开始](#快速开始)
  - [准备编译环境](#准备编译环境)
  - [获取SDK](#获取sdk)
  - [编译](#编译)
  - [SD卡烧录](#sd卡烧录)
- [其他编译注意事项](#其他编译注意事项)

<br>

# 项目简介
- 本仓库提供芯片`CV181x`和`CV180x`两个系列芯片(兼容`SG2002-CV1812CP`,`SG2000-CV1812H`)的软件开发包(SDK)。
- 主要适用于官方EVB和Milkv-Duo、Milkv Duo256。

<br>

## 硬件资料
- [《CV180xB 开发板硬件指南》](https://sophon-file.sophon.cn/sophon-prod-s3/drive/23/03/14/14/CV180xB_EVB%E6%9D%BF%E7%A1%AC%E4%BB%B6%E6%8C%87%E5%8D%97_V1.0.pdf)
- [《CV180xC 开发板硬件指南》](https://sophon-file.sophon.cn/sophon-prod-s3/drive/23/03/18/18/CV180xC_EVB%E6%9D%BF%E7%A1%AC%E4%BB%B6%E6%8C%87%E5%8D%97_V1.0.pdf)
- [《CV181xC 开发板硬件指南》](https://sophon-file.sophon.cn/sophon-prod-s3/drive/23/03/15/14/CV181xC_QFN_EVB%E6%9D%BF%E7%A1%AC%E4%BB%B6%E6%8C%87%E5%8D%97_V1.0.pdf)
- [《CV181xH 开发板硬件指南》](https://sophon-file.sophon.cn/sophon-prod-s3/drive/23/03/15/15/CV181xH_EVB%E6%9D%BF%E7%A1%AC%E4%BB%B6%E6%8C%87%E5%8D%97_V1.0.pdf)
- [《SDK 开发文档汇总》](https://developer.sophgo.com/thread/471.html)
- [《HDK 开发文档汇总》](https://developer.sophgo.com/thread/472.html)

<br>

## 芯片规格
- [芯片产品简介](https://www.sophgo.com/product/index.html)
- [SG2000](https://milkv.io/zh/chips/sg2000)
- [SG2002](https://milkv.io/zh/chips/sg2002)
<br><br>

# SDK目录结构
```
.
├── build               // 编译目录，存放编译脚本以及各board差异化配置
├── buildroot-2021.05   // buildroot开源工具
├── freertos            // freertos系统
├── fsbl                // fsbl启动固件，prebuilt形式存在
├── install             // 执行一次完整编译后，各image的存放路径
├── isp_tuning          // 图像效果调试参数存放路径
├── linux_5.10          // 开源linux内核
├── middleware          // 自研多媒体框架，包含so与ko
├── opensbi             // 开源opensbi库
├── ramdisk             // 存放最小文件系统的prebuilt目录
└── u-boot-2021.10      // 开源uboot代码
```

<br><br>

# 快速开始

## 准备编译环境
- 在虚拟机上安装一个ubuntu系统，或者使用本地的ubuntu系统，推荐`Ubuntu 22.04 LTS`
- 安装串口工具： `mobarXterm` 或者 `xshell` 或者其他
- 安装编译依赖的工具:
```
sudo apt install -y pkg-config build-essential ninja-build automake autoconf libtool wget curl git gcc libssl-dev bc slib squashfs-tools android-sdk-libsparse-utils jq python3-distutils scons parallel tree python3-dev python3-pip device-tree-compiler ssh cpio fakeroot libncurses5 flex bison libncurses5-dev genext2fs rsync unzip dosfstools mtools tcl openssh-client cmake expect
```
*注意：cmake版本最低要求3.16.5*

## 获取SDK
```
git clone https://github.com/cjiio/foxiopi.git --depth=1
cd foxiopi
```

## 编译
- 以 `cv1812cp_foxio_tiny_spinand`为例
```
source build/cvisetup.sh
defconfig cv1812cp_foxio_tiny_spinand
build_all
```
- 编译成功后可以在`install`目录下看到生成的image

## SD卡烧录
- 接好开发板的串口线
- 将SD卡格式化成FAT32格式
- 将`install`目录下的image放入SD卡根目录
```
.
├── boot.emmc
├── cfg.emmc
├── fip.bin
├── fw_payload_uboot.bin
├── rootfs.emmc
└── system.emmc
```
- 将SD卡插入的SD卡槽中
- 将平台重新上电，开机自动进入烧录，烧录过程log如下：
```
Hit any key to stop autoboot:  0
## Resetting to default environment
Start SD downloading...
mmc1 : finished tuning, code:60
465408 bytes read in 11 ms (40.3 MiB/s)
mmc0 : finished tuning, code:27
switch to partitions #1, OK
mmc0(part 1) is current device

MMC write: dev # 0, block # 0, count 2048 ... 2048 blocks written: OK in 17 ms (58.8 MiB/s)

MMC write: dev # 0, block # 2048, count 2048 ... 2048 blocks written: OK in 14 ms (71.4 MiB/s)
Program fip.bin done
mmc0 : finished tuning, code:74
switch to partitions #0, OK
mmc0(part 0) is current device
64 bytes read in 3 ms (20.5 KiB/s)
Header Version:1
2755700 bytes read in 40 ms (65.7 MiB/s)

MMC write: dev # 0, block # 0, count 5383 ... 5383 blocks written: OK in 64 ms (41.1 MiB/s)
64 bytes read in 4 ms (15.6 KiB/s)
Header Version:1
13224 bytes read in 4 ms (3.2 MiB/s)

MMC write: dev # 0, block # 5760, count 26 ... 26 blocks written: OK in 2 ms (6.3 MiB/s)
64 bytes read in 4 ms (15.6 KiB/s)
Header Version:1
11059264 bytes read in 137 ms (77 MiB/s)

MMC write: dev # 0, block # 17664, count 21600 ... 21600 blocks written: OK in 253 ms (41.7 MiB/s)
64 bytes read in 3 ms (20.5 KiB/s)
Header Version:1
4919360 bytes read in 65 ms (72.2 MiB/s)

MMC write: dev # 0, block # 158976, count 9608 ... 9608 blocks written: OK in 110 ms (42.6 MiB/s)
64 bytes read in 4 ms (15.6 KiB/s)
Header Version:1
10203200 bytes read in 128 ms (76 MiB/s)

MMC write: dev # 0, block # 240896, count 19928 ... 19928 blocks written: OK in 228 ms (42.7 MiB/s)
Saving Environment to MMC... Writing to MMC(0)... OK
mars_c906#
```
- 烧录成功，拔掉SD卡，重新给板子上电，进入系统

<br><br>


# 其他编译注意事项
- 如果您想尝试在以上两种环境之外的环境下编译本 SDK，下面是可能需要注意的事项，仅供参考。
- 如果您希望使用 WSL 执行编译，则构建镜像时会遇到一个小问题，WSL 中的 $PATH 具有 Windows 环境变量，其中路径之间包含一些空格。
- 要解决此问题，您需要更改 `/etc/wsl.conf` 文件并添加以下行：
```
[interop]
appendWindowsPath = false
```
- 然后需要使用 `wsl.exe --reboot` 重新启动 WSL。再运行编译命令。
- 要恢复 `/etc/wsl.conf` 文件中的此更改，请将 `appendWindowsPath` 设置为 `true`。 要重新启动 WSL，您可以使用 `Windows PowerShell` 命令 `wsl.exe --shutdown`，然后使用`wsl.exe`，之后 Windows 环境变量在 `$PATH` 中再次可用。
