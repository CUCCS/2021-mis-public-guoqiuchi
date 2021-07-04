# Android 缺陷应用漏洞攻击实验 #
## 实验目的 ##
- 理解 Android 经典的组件安全和数据安全相关代码缺陷原理和漏洞利用方法；
- 掌握 Android 模拟器运行环境搭建和 ADB 使用；

## 实验环境 ##
- [Android-InsecureBankv2](https://github.com/c4pr1c3/Android-InsecureBankv2)

## 实验要求 ##
- 详细记录实验环境搭建过程；
- 至少完成以下 实验 ：
 - Developer Backdoor
 - Insecure Logging
 - Android Application patching + Weak Auth
 - Exploiting Android Broadcast Receivers
 - Exploiting Android Content Provider

## 实验过程 ##
### 实验环境 ###
- 下载并安装Android Studio
- 下载并安装Android SDK
- SDK Manager 的 SDK Platforms 选项卡选择不同版本的 SDK 下载安装
- 在AVD Manager标签下安装相应镜像后可以运行虚拟机
  ![](imgs/4.png)
  
- ADB使用
  - 使用adb连接虚拟机   
    - 开启模拟器的开发者模式：在模拟器中找到设置，点击版本号“Build Number”  
   
    - 点击进入"开发者选项"，选择"允许USB调试"，连接USB后启用调试模式。 

### 渗透实验过程 ###
### 1.Developer Backdoor ###
- 克隆Android-InsecureBankv2到本地
- 下载JADX反编译器：https://github.com/skylot/jadx   
- 下载dex2jar：https://bitbucket.org/pxb1988/dex2jar/downloads
- 使用命令解压原来下载的Insecure Bankv2.apk文件的内容：unzip InsecureBankv2.apk        
  
  得到classes.dex文件
- Android-InsecureBankv2应用程序中存在的开发人员后门的反编译代码，该程序允许用户名为devadmin的用户与所有其他用户相比使用不同的终结点。
- 发现任何用户都可以使用用户名devadmin搭配任何密码登录应用程序，而不管

- 将InsecureBankv2.apk文件复制到Android SDK中的“platform-tools”文件夹，然后将下载的Android-InsecureBankv2应用程序推送到模拟器：./adb install InsecureBankv2.apk     
  
- 在模拟器上启动已安装的InsecureBankv2应用程序         
  !(imgs/18.png)
- 以用户名为sheldon，密码sheldon@168#，进行登录  
  
- 用lagcat查看日志可以看到登录的用户名和密码信息             
  
- 导航到 “Change Password”页面并输入新密码。    
  ![](imgs/21.png) 
  ![](imgs/22.png)    
- 用lagcat查看日志可以看到修改密码的信息          
  

### 3.Android Application patching + Weak Auth ###
- 下载apktool: http://ibotpeaches.github.io/Apktool/      
  载SignApk: https://github.com/appium/sign
- d模拟器中启动新安装的Insecure Bankv2应用程序。显示中用户提供了一个附加的“Create User”按钮，此按钮仅用于管理员用户，对于普通用户以前不可见，这样一来，普通用户相当于获取了管理员的权限

- 在shell中输入：  
  ```content query --uri content://com.android.insecurebankv2.TrackUserContentProvider/trackerusers```     
  发现所有用户的登录历史都是以未加密的方式存储在设备上的，得到用户名后，可进行其它的攻击

## 参考文献 ##
- [Android Studio下载](https://developer.android.com/studio/)
- [实验教程](https://github.com/c4pr1c3/Android-InsecureBankv2)
- [apktool安装指南](http://ibotpeaches.github.io/Apktool/install/)

  