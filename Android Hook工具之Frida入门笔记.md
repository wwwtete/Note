Frida是一款基于Python + JavaScript的Hook调试框架，可以将自己编写的JavaScript代码注入到Windows，MACOS，Linux， iOS，Android和QNX 的应用中从而进行Hook，其实Frida功能不仅仅是Hook，还包括以下这些功能:

- 访问进程的内存
- 在应用程序运行时覆盖一些功能
- 从导入的类中调用函数
- 在堆上查找对象实例并使用这些对象实例
- Hook，跟踪和拦截函数等等

> Frida官网地址：https://www.frida.re.  

 Frida可以运行在多个平台上，这次主要讲解使用MacOS当宿主机的使用方式
 - 宿主机系统：Mac OS
 - Android手机: 已Root过的Android 手机或使用Android模拟器也可以（PS:我使用的华为荣耀6p的Android 6.0系统，模拟器没用过不知道会不会有坑）
 - Frida: Frida有多重安装方式,这里主要记录一下常用的两种方式。
 方式一:  直接通过Pip安装Frida，一般Mac系统上都会有Python环境，如果没有则需要先安装python（最好用最新的版本）安装完Python后继续安装pip，打开终端输入`sudo easy_install pip`命令进行安装
``` stylus
sudo easy_install pip
Password:
```
安装完pip后就可以安装Frida了，直接输入`sudo pip install frida`即可(安装时间比较长耐心等待，最好使用VPN)

``` stylus
sudo pip install frida
```

方式二: 直接下载对应平台的Python版本的安装包，比如：当前系统是Mac OS-10.12,Python是2.7，那么就应该下载:frida-10.6.52-py2.7-macosx-10.12-intel.egg ,下载完成后直接安装即可  
下载地址: https://pypi.python.org/pypi/frida  
 
安装完成后，直接在终端中输入`frida-ps`命令查看，如果能显示当前系统进程则证明安装成功
``` stylus
frida-ps
 PID  Name
----  ---------------------------------------------
 416  AirPlayUIAgent
 596  Android Studio
 551  AppleSpell.service
 529  CoreServicesUIAgent
 264  Dock
 266  Finder
 402  FolderActionsDispatcher
 553  Google Chrome
 505  LaterAgent
 517  QQ
 530  QQ jietu plugin
```
- Frida-server: 直接去官网下载:https://github.com/frida/frida/releases 对应的版本即可，注意：Frida-server的版本必须跟你宿主机的Frida版本一致,比如我宿主机Frida的版本是10.6.52，Android手机是arm的，那么应该下载：rida-server-10.6.52-android-arm.xz 文件。  
下载后解压文件，并将文件重命名为: `frida-server`, 重命名完成后使用`adb push`命令推送到手机中
``` stylus
adb push frida-server /data/local/tmp/
```
推送完成后将frida-sever赋予执行的权限，并运行Frida-server，使用以下命令:
``` stylus
adb shell
su
cd /data/local/tmp/
chmod 777 frida-server
./frida-server
```
注1： 如果frida-server没有启动，查看一下你是否使用的是Root用户来启动，如果使用Root用户则应该是`#`
注2： 如果要启动frida-server作为后台进程、可以使用这个命令`./frida-server &`
正常启动后，另开一个终端，使用`frida-ps -U`命令检查Frida是否正常运行，如果正常运行则会列出Android设备上当前正在运行的进程.
``` stylus
frida-ps -U
  PID  Name
-----  ------------------------------------------
 3835  31:0
 3724  HwCamCfgSvr
 3954  adbd
 5011  android.process.acore
 5029  android.process.media
 3739  bastetd
 3736  check_longpress
 3764  check_longpress
13962  com.UCMobile:channel
14462  com.UCMobile:push
```
参数-U 代表USB，意思让Frida检查USB设备，使用`frida-ps -R` 也可以，但是需要进行转发。执行`adb forward tcp:27042 tcp:27042`后执行`frida-ps -R`也可以看到手机上的进程.   
到此为止，Frida工作环境已经准备好了，下一篇文章介绍一下Frida框架的基本使用方式