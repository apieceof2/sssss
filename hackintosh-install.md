# 黑苹果安装

## 安装准备

### 需要下载的
* Mac 懒人包
* MacDirve 
* HFS + for windows
* Notepad ++
* winPE
* AIDA64
* 变色龙
* easyBCD
* 变色龙Extra
* MultiBeast
* Kext Wizard
* Chameleon Wizard
* Bootice 1.3
* 硬盘安装助手

### U盘
    U盘A：安装winPE，以防安装mac os 以后win无法启动
    U盘B：mac os安装盘

### 电脑A
用来修改
1. MacDrive 8
2. HFS+for windows 用来打开修改U盘B中的镜像
3. Notepad++ 

### 电脑B
用来安装mac
1. 将BIOS启动模式设置成传统模式
2. SATA设置ACHI
3. 关闭intel CPU的VT-D功能
4. 传统BIOS+MBR安装好win
5. __使用U盘A中的DiskGenius为电脑分出mac区,当然了，不能格式化__
6. 使用AIDA64找出硬件相关信息

### 变色龙安装
1. 安装变色龙到电脑B,用easyBCD挂载
2. 载win下使用硬盘安装助手,Bootice1.3为U盘B安装mac懒人镜像
3. 安装了HFS + for windows 的电脑A就可以打开U盘B
4. 检查是否有Extra,如果没有就把下载好的放进去
5. 进入SLE,备份并删除以NV,Geforce,AMD,ATI,IntelHD,IOBluetooth开头的文件
6. 打开Extra下的org.chameleon.Boot.plsit,修改GE=Yes

### 安装苹果系统
以HDD而不是UEFI启动U盘
* HDD启动名:U盘名称或型号
* UEFI启动名:UEFI+U盘名称或型号






