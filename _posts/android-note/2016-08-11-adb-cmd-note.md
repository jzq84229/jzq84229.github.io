---
layout: post
title: adb命令
category: Android
tags:
keywords:
description:
---

### adb命令
adb shell pm list packages：列出所有的包名。  
adb shell dumpsys package：列出所有的安装应用的信息  
dumpsys package com.android.XXX：查看某个包的具体信息  

### 查看logcat
查看和跟踪系统日志缓冲区的命令logcat的一般用法是：
```
[adb] logcat [<option>] ... [<filter-spec>] ...
```

将日志保存到文件
```
logcat -v time > d:\log\log1.txt
```

log过滤
```
logcat | grep MyAppName
```

清除log
```
logcat -c
```

### adb查看cpu信息
```
adb shell
cd proc
cat cpuinfo
```

### adb无线连接调试app
1. 使用usb连接手机
2. 输入命令：adb tcpip 5555 系统会返回`restarting in TCP mode port:5555`
3. 查看手机ip如：192.168.0.120 输入命令：`adb connect 192.168.0.120:5555` 系统返回:`adb connect 192.168.0.120:5555`
手机已通过wifi连接成功，可以拔掉usb即可使用adb命令
4. 若要退出无线模式，输入：adb usb

### 利用run-as命令在不root情况下读取data下面的数据
```
adb shell
run-as com.package
```

```
E:\Android-sdk\android-sdk-windows\platform-tools>adb android list targets
Android Debug Bridge version 1.0.39
Revision 3db08f2c6889-android
Installed as E:\Android-sdk\android-sdk-windows\platform-tools\adb.exe

global options:
 -a         listen on all network interfaces, not just localhost
 -d         use USB device (error if multiple devices connected)
 -e         use TCP/IP device (error if multiple TCP/IP devices available)
 -s SERIAL
     use device with given serial number (overrides $ANDROID_SERIAL)
 -p PRODUCT
     name or path ('angler'/'out/target/product/angler');
     default $ANDROID_PRODUCT_OUT
 -H         name of adb server host [default=localhost]
 -P         port of adb server [default=5037]
 -L SOCKET  listen on given socket for adb server [default=tcp:localhost:5037]

general commands:
 devices [-l]             list connected devices (-l for long output)
 help                     show this help message
 version                  show version num

networking:
 connect HOST[:PORT]      connect to a device via TCP/IP [default port=5555]
 disconnect [HOST[:PORT]]
     disconnect from given TCP/IP device [default port=5555], or all
 forward --list           list all forward socket connections
 forward [--no-rebind] LOCAL REMOTE
     forward socket connection using:
       tcp:<port> (<local> may be "tcp:0" to pick any open port)
       localabstract:<unix domain socket name>
       localreserved:<unix domain socket name>
       localfilesystem:<unix domain socket name>
       dev:<character device name>
       jdwp:<process pid> (remote only)
 forward --remove LOCAL   remove specific forward socket connection
 forward --remove-all     remove all forward socket connections
 ppp TTY [PARAMETER...]   run PPP over USB
 reverse --list           list all reverse socket connections from device
 reverse [--no-rebind] REMOTE LOCAL
     reverse socket connection using:
       tcp:<port> (<remote> may be "tcp:0" to pick any open port)
       localabstract:<unix domain socket name>
       localreserved:<unix domain socket name>
       localfilesystem:<unix domain socket name>
 reverse --remove REMOTE  remove specific reverse socket connection
 reverse --remove-all     remove all reverse socket connections from device

file transfer:
 push LOCAL... REMOTE
     copy local files/directories to device
 pull [-a] REMOTE... LOCAL
     copy files/dirs from device
     -a: preserve file timestamp and mode
 sync [DIR]
     copy all changed files to device; if DIR is "system", "vendor", "oem",
     or "data", only sync that partition (default all)
     -l: list but don't copy

shell:
 shell [-e ESCAPE] [-n] [-Tt] [-x] [COMMAND...]
     run remote shell command (interactive shell if no command given)
     -e: choose escape character, or "none"; default '~'
     -n: don't read from stdin
     -T: disable PTY allocation
     -t: force PTY allocation
     -x: disable remote exit codes and stdout/stderr separation
 emu COMMAND              run emulator console command

app installation:
 install [-lrtsdg] PACKAGE
 install-multiple [-lrtsdpg] PACKAGE...
     push package(s) to the device and install them
     -l: forward lock application
     -r: replace existing application
     -t: allow test packages
     -s: install application on sdcard
     -d: allow version code downgrade (debuggable packages only)
     -p: partial application install (install-multiple only)
     -g: grant all runtime permissions
 uninstall [-k] PACKAGE
     remove this app package from the device
     '-k': keep the data and cache directories

backup/restore:
 backup [-f FILE] [-apk|-noapk] [-obb|-noobb] [-shared|-noshared] [-all] [-syste
m|-nosystem] [PACKAGE...]
     write an archive of the device's data to FILE [default=backup.adb]
     package list optional if -all/-shared are supplied
     -apk/-noapk: do/don't back up .apk files (default -noapk)
     -obb/-noobb: do/don't back up .obb files (default -noobb)
     -shared|-noshared: do/don't back up shared storage (default -noshared)
     -all: back up all installed applications
     -system|-nosystem: include system apps in -all (default -system)
 restore FILE             restore device contents from FILE

debugging:
 bugreport [PATH]
     write bugreport to given PATH [default=bugreport.zip];
     if PATH is a directory, the bug report is saved in that directory.
     devices that don't support zipped bug reports output to stdout.
 jdwp                     list pids of processes hosting a JDWP transport
 logcat                   show device log (logcat --help for more)

security:
 disable-verity           disable dm-verity checking on userdebug builds
 enable-verity            re-enable dm-verity checking on userdebug builds
 keygen FILE
     generate adb public/private key; private key stored in FILE,
     public key stored in FILE.pub (existing files overwritten)

scripting:
 wait-for[-TRANSPORT]-STATE
     wait for device to be in the given state
     State: device, recovery, sideload, or bootloader
     Transport: usb, local, or any [default=any]
 get-state                print offline | bootloader | device
 get-serialno             print <serial-number>
 get-devpath              print <device-path>
 remount
     remount /system, /vendor, and /oem partitions read-write
 reboot [bootloader|recovery|sideload|sideload-auto-reboot]
     reboot the device; defaults to booting system image but
     supports bootloader and recovery too. sideload reboots
     into recovery and automatically starts sideload mode,
     sideload-auto-reboot is the same but reboots after sideloading.
 sideload OTAPACKAGE      sideload the given full OTA package
 root                     restart adbd with root permissions
 unroot                   restart adbd without root permissions
 usb                      restart adb server listening on USB
 tcpip PORT               restart adb server listening on TCP on PORT

internal debugging:
 start-server             ensure that there is a server running
 kill-server              kill the server if it is running
 reconnect                kick connection from host side to force reconnect
 reconnect device         kick connection from device side to force reconnect

environment variables:
 $ADB_TRACE
     comma-separated list of debug info to log:
     all,adb,sockets,packets,rwx,usb,sync,sysdeps,transport,jdwp
 $ADB_VENDOR_KEYS         colon-separated list of keys (files or directories)
 $ANDROID_SERIAL          serial number to connect to (see -s)
 $ANDROID_LOG_TAGS        tags to be used by logcat (see logcat --help)

E:\Android-sdk\android-sdk-windows\platform-tools>
```

### adb常用命令
1. 获取序列号：
`adb get-serialno`
2. 查看连接计算机的设备：
`adb devices`
3. 重启机器：
`adb reboot`
4. 重启到bootloader，即刷机模式：
`adb reboot bootloader`
5. 重启到recovery，即恢复模式：
`adb reboot recovery`
6. 查看log：
`adb logcat`
7. 终止adb服务进程：
`adb kill-server`
8. 重启adb服务进程：
`adb start-server`
9. 获取机器MAC地址：
`adb shell  cat /sys/class/net/wlan0/address`
10. 获取CPU序列号：
`adb shell cat /proc/cpuinfo`
11. 安装APK：
`adb install <apkfile> //比如：adb install baidu.apk`
12. 保留数据和缓存文件，重新安装apk：
`adb install -r <apkfile> //比如：adb install -r baidu.apk`
13. 安装apk到sd卡：
`adb install -s <apkfile> // 比如：adb install -s baidu.apk`
14. 卸载APK：
`adb uninstall <package> //比如：adb uninstall com.baidu.search`
15. 卸载app但保留数据和缓存文件：
`adb uninstall -k <package> //比如：adb uninstall -k com.baidu.search`
16. 启动应用：
`adb shell am start -n <package_name>/.<activity_class_name>`
17. 查看设备cpu和内存占用情况：
`adb shell top`
18. 查看占用内存前6的app：
`adb shell top -m 6`
19. 刷新一次内存信息，然后返回：
`adb shell top -n 1`
20. 查询各进程内存使用情况：
`adb shell procrank`
21. 杀死一个进程：
`adb shell kill [pid]`
22. 查看进程列表：
`adb shell ps`
23. 查看指定进程状态：
`adb shell ps -x [PID]`
24. 查看后台services信息：
`adb shell service list`
25. 查看当前内存占用：
`adb shell cat /proc/meminfo`
26. 查看IO内存分区：
`adb shell cat /proc/iomem`
27. 将system分区重新挂载为可读写分区：
`adb remount`
28. 从本地复制文件到设备：
`adb push <local> <remote>`
29. 从设备复制文件到本地：
`adb pull <remote>  <local>`
30. 列出目录下的文件和文件夹，等同于dos中的dir命令：
`adb shell ls`
31. 进入文件夹，等同于dos中的cd 命令：
`adb shell cd <folder>`
32. 重命名文件：
`adb shell rename path/oldfilename path/newfilename`
33. 删除system/avi.apk：
`adb shell rm /system/avi.apk`
34. 删除文件夹及其下面所有文件：
`adb shell rm -r <folder>`
35. 移动文件：
`adb shell mv path/file newpath/file`
36. 设置文件权限：
`adb shell chmod 777 /system/fonts/DroidSansFallback.ttf`
37. 新建文件夹：
`adb shell mkdir path/foldelname`
38. 查看文件内容：
`adb shell cat <file>`
39. 查看wifi密码：
`adb shell cat /data/misc/wifi/*.conf`
40. 清除log缓存：
`adb logcat -c`
41. 查看bug报告：
`adb bugreport`
42. 获取设备名称：
`adb shell cat /system/build.prop`
43. 查看ADB帮助：
`adb help`
44. 跑monkey：
`adb shell monkey -v -p your.package.name 500`


## Adb 高级命令
- am – ActivityManager 命令
- pm – PackageManager命令
- wm – WindowManager 命令
- content – ContentProvider命令

### am命令
- am 子命令
- 可以启动Activity，Service  
- 可以停止一个应用  
- 可以发送broadcast  
- 可以监控activity的状态  
```
am start –n <ComponentName>
am start –a <Action> -c <Category> -e<Extra>
```
例：
```
am start –n com.android.settings //启动系统设置
am start –a com.android.intent.action.MAIN –c com.android.intent.category.HOME //启动Home页
am force-stop <package>
am start -a android.intent.action.VIEW -d http://www.baidu.com
am start -a android.intent.action.CALL -d tel:12345
am startservice –n <ComponentName>
am broadcast …
am force-stop <package>
```


### pm命令
- pm install – 安装一个apk (可以用adb install)
- pm uninstall [-k] – 删除一个apk (可以用adb uninstall)
- pm list — 列出一系列包，Feature或者permission等
- pm path — 获得某个包的apk文件路径
- pm clear — 清除某个应用的数据
- pm enable —启用某个组件
- pm disable — 禁用某个组件

### wm命令
- wm (WindowManager)命令，主要查看屏幕的信息
- wm size — 获取屏幕尺寸
- wm density – 获取屏幕密度
- wm overscan – 过扫描

### content命令
- 操作ContentProvider的命令
- content insert – 向content provider中插入数据
- content query – 请求content provider中的数据
- content update – 更新content provider中的数据
- content delete – 删除 content provider中的数据
- \*数据值之间用冒号(:)分隔

### settings命令
```
settings put global my_key “hello”
settings get global my_key
```

### media媒体控制命令
```
media dispatch <PLAY, PAUSE, STOP …>
```
模拟一个媒体按键的发送
