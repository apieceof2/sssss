# 黑苹果爬贴记录

## 引导方式
1. UEFI
2. BIOS

## 引导工具
1. Chameleon：历史悠久，功能没有clover好，适合BIOS，老手使用
2. Clover：功能强大，很多问题可以通过调试config.plist来解决，支持UEFI/BIOS，适合新手

## 镜像版本
1. 懒人版
    优点：支持MBR/GBT分区表的硬盘，__方便修改安装盘中的内容__
	缺点：不会生成RecoveryHD恢复分区

2. 原版
	优点：安装时类似于白果的安装，高大上，有recoveryHD恢复分区
	缺点：不能修改安装盘中的内容 __只能安装到GPT分区硬盘__

3.整合版


### 说明
虽然原版不能修改安装盘内的内容，但是遇到很多的错误的时候都可能用config.plist来解决

## 引导+镜像搭配方案
1. BIOS+MBR：变色龙+懒人版
2. UEFI+GPT：clover+原版或clover+懒人版
3. BIOS+GPT：clover/变色龙 + 懒人版或原版
4. UEFI+MBR：转成GPT，使用第二种。或者clover+懒人版。或者改成BIOS驱动
*主流搭配为前两种*

>我的电脑现在应该是BIOS+MBR的类型，可以先装这个玩一玩，然后，等到买到硬盘以后，设置UEFI and legacy，硬盘格成GPT，做第一种


## 专有名词
1. kext: Kernel Extensions 驱动
2. SLE: /System/Library/Extensions
3. EE: /Extra/Extensions 变色龙独有，用于放入其他第三方驱动
4. boot.plist/变色龙配置文件/变色龙的plist：org.chameleon.Boot.plist
5. kextx/10.9: /EFI/Clover/Kexts/10.9 这个是clover的第三方驱动目录，对应变色龙的EE，10.9是版本号
6. boot with injected kexts 或者 boot without caches: 就是-v -f
7. GE=YES/NO: 变色龙显卡检测，在org.chameleon.Boot.plist中加入
```
<key>GraphicsEnabler</key>
<string>Yes</string>
```

>2017-7-14 23:17:17

