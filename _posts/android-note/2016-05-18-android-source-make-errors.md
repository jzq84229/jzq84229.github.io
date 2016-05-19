---
layout: post
title: android源码编译错误
category: Android
tags: Android
keywords: 
description:
---

系统版本： ubuntu 14.04 64位
android版本：  4.0.1 r1
jdk版本：  1.6_49 (jdk版本必须为1.6版本，否则提示jdk版本错误)

编译源码需要用到的工具包：
以下为ubuntu 14.04 64位系统的包，其他版本详见android官网[http://source.android.com/source/initializing.html](http://source.android.com/source/initializing.html)
    sudo apt-get install git-core gnupg flex bison gperf build-essential \
          zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 \
          lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev ccache \
          libgl1-mesa-dev libxml2-utils xsltproc unzip


- make: \*\*\* [out/host/linux-x86/obj/EXECUTABLES/emugen_intermediates/main.o] error 1
> 解决办法： 需要在  development/tools/emulator/opengl/host/tools/emugen/main.cpp  
        在声明中增加一条头文件声明
        #include <getopt.h>
        
- make: \*\*\* [out/host/linux-x86/obj/EXECUTABLES/aapt_intermediates/AaptAssets.o] error 1
> 解决办法:  frameworks/base/tools/aapt/Android.mk
       在第31行增加:
       LOCAL_CFLAGS += -Wno-format-y2k -fpermissive
       
- make: \*\*\* [out/host/linux-x86/obj/STATIC_LIBRARIES/libutils_intermediates/AssetManager.o] error 1
> 解决办法：在 frameworks/base/libs/utils/Android.mk
      在第60行后面增加-fpermissive：
      LOCAL_CFLAGS += -DLIBUTILS_NATIVE=1 $(TOOL_CFLAGS) -fpermissive
      
- make: *** [out/host/linux-x86/obj/EXECUTABLES/aapt_intermediates/aapt] error 1
      /usr/bin/ld: cannot find -lz
      collect2: error: ld returned 1 exit status
      make: *** [out/host/linux-x86/obj/EXECUTABLES/aapt_intermediates/aapt] error 1
> 解决方法： 
      sudo apt-get install lib32z1-dev
      
- make: *** [out/host/linux-x86/obj/EXECUTABLES/grxmlcompile_intermediates/grxmlcompile.o] Error 1
> 解决办法：cd external/srec
   复制拷贝下面的命令到终端:
     wget "https://github.com/CyanogenMod/android_external_srec/commit/4d7ae7b79eda47e489669fbbe1f91ec501d42fb2.diff"
     patch -p1 < 4d7ae7b79eda47e489669fbbe1f91ec501d42fb2.diff
    rm -f 4d7ae7b79eda47e489669fbbe1f91ec501d42fb2.diff
    cd ../..
    
- make: *** [out/host/linux-x86/obj/SHARED_LIBRARIES/libdvm_intermediates/native/dalvik_system_Zygote.o] Error 1
> 解决方法
        add #include <sys/resource.h> to dalvik/vm/native/dalvik_system_Zygote.cpp
        
- make: *** [out/host/linux-x86/obj/STATIC_LIBRARIES/libOpenglCodecCommon_intermediates/GLSharedGroup.o] Error 1
> 解决办法： $vim development/tools/emulator/opengl/Android.mk
        Add '-fpermissive' to line 25:
        EMUGL_COMMON_CFLAGS := -DWITH_GLES2 -fpermissive
        
- make: *** [out/host/linux-x86/obj/lib/libOpenglRender.so] Error 1
        <built-in>:0:0: note: this is the location of the previous definition
        host StaticLib: libOpenglCodecCommon (out/host/linux-x86/obj/STATIC_LIBRARIES/libOpenglCodecCommon_intermediates/libOpenglCodecCommon.a)
        host SharedLib: libOpenglRender (out/host/linux-x86/obj/lib/libOpenglRender.so)
        /usr/bin/ld: cannot find -lX11
        collect2: error: ld returned 1 exit status
        make: *** [out/host/linux-x86/obj/lib/libOpenglRender.so] Error 1
> 解决方法:
        sudo ln -s /usr/lib/i386-linux-gnu/libX11.so.6 /usr/lib/i386-linux-gnu/libX11.so

- make: *** [out/host/linux-x86/obj/EXECUTABLES/emulator_renderer_intermediates/emulator_renderer] Error 1
        /usr/bin/ld: out/host/linux-x86/obj/EXECUTABLES/emulator_renderer_intermediates/main.o: undefined reference to symbol 'XInitThreads'
        //usr/lib/i386-linux-gnu/libX11.so.6: error adding symbols: DSO missing from command line
        collect2: error: ld returned 1 exit status
        make: *** [out/host/linux-x86/obj/EXECUTABLES/emulator_renderer_intermediates/emulator_renderer] Error 1
> 解决办法： 
vi development/tools/emulator/opengl/host/renderer/Android.mk
        Add new entry 'LOCAL_LDLIBS += -lX11' after line 6 as shown:
        LOCAL_SRC_FILES := main.cpp
        LOCAL_CFLAGS    += -O0 -g
        LOCAL_LDLIBS += -lX11
        #ifeq ($(HOST_OS),windows)
        #LOCAL_LDLIBS += -lws2_32 

- /usr/include/zlib.h:34:19: fatal error: zconf.h: No such file or directory
        /usr/include/zlib.h:34:19: fatal error: zconf.h: No such file or directory
        #include "zconf.h"
> 解决办法：
        sudo ln -s /usr/include/x86_64-linux-gnu/zconf.h /usr/include

- make: *** [out/host/linux-x86/obj/EXECUTABLES/obbtool_intermediates/Main.o] error 1
> 解决办法：
        是由于版本太高，在配置环境的时候，gcc安装了高版本，所以gcc版本太高导致，需要降低gcc版本级别。
        ubuntu 32bit系统下安装gcc 4.4的最好方法是仅用以下两条命令，不需要其它命令，否则编译时可能会出错。
        sudo apt-get install gcc-4.4
        sudo apt-get install g++-4.4
        操作过程见：
        gcc降级：
        sudo rm -rf /usr/bin/gcc
        sudo ln -s /usr/bin/gcc-4.4 /usr/bin/gcc
        gcc -v
        g++降级
        sudo rm -rf /usr/bin/g++
        sudo ln -s /usr/bin/g++-4.4 /usr/bin/g++
        g++ -v

- g++: selected multilib '32' not installed
        host Executable: obbtool (out/host/linux-x86/obj/EXECUTABLES/obbtool_intermediates/obbtool)
        g++: selected multilib '32' not installed
        make: *** [out/host/linux-x86/obj/EXECUTABLES/obbtool_intermediates/obbtool] Error 1
> 解决办法：
        sudo apt-get install g++-4.4-multilib
        
- make: *** [out/host/linux-x86/obj/lib/libGLES_CM_translator.so] Error 1
        /usr/bin/ld: cannot find -lGL
        collect2: ld returned 1 exit status
        make: *** [out/host/linux-x86/obj/lib/libGLES_CM_translator.so] Error 1
> 解决办法：
        sudo ln -s /usr/lib/i386-linux-gnu/mesa/libGL.so.1.2 /usr/lib/libGL.so 