---
layout: post
title: linux笔记
category: 资源
tags: linux
keywords: linux
description:
---

# linux笔记



## 压缩解压缩
- 总结

|压缩文件后缀|解压命令|
|:---|:---|
| \*.tar | tar –xvf 解压|
| \*.gz | gzip -d或者gunzip 解压|
| \*.tar.gz和*.tgz | tar –xzf 解压|
| \*.bz2 | bzip2 -d或者用bunzip2 解压|
| \*.tar.bz2 | tar –xjf 解压|
| \*.Z | uncompress 解压|
| \*.tar.Z |tar –xZf 解压|
| \*.rar | unrar e解压|
| \*.zip | unzip 解压|

- tar

  1. Linux下最常用的打包程序就是tar了，使用tar程序打出来的包我们常称为tar包，tar包文件的命令通常都是以.tar结尾的。
  2. **tar调用gzip**
  gzip是GNU组织开发的一个压缩程序，.gz结尾的文件就是gzip压缩的结果。与gzip相对的解压程序是gunzip。tar中使用-z这个参数来调用gzip。
  3. **tar调用bzip2**
  bzip2是一个压缩能力更强的压缩程序，.bz2结尾的文件就是bzip2压缩的结果。与bzip2相对的解压程序是bunzip2。tar中使用-j这个参数来调用bzip2。
  4. **tar调用compress**
  compress也是一个压缩程序，但是好象使用compress的人不如gzip和bzip2的人多。.Z结尾的文件就是bzip2压缩的结果。与compress相对的解压程序是uncompress。tar中使用-Z这个参数来调用compress。

- zip压缩

  1. 把一个文件abc.txt和一个目录dir压缩为yasuo.zip

          zip -r yasuo.zip abc.txt dir1

- unzip解压缩
  1. 把yasuo.zip解压

          unzip yasuo.zip

  2. 我当前目录下有abc1.zip，abc2.zip和abc3.zip，我想一起解压缩它们：

          unzip abc\?.zip

    > 注释：?表示一个字符，如果用*表示任意多个字符。

  3. 我有一个很大的压缩文件large.zip，我不想解压缩，只想看看它里面有什么：

          unzip -v large.zip

  4. 我下载了一个压缩文件large.zip，想验证一下这个压缩文件是否下载完全了

          unzip -t large.zip

  5. 我用-v选项发现music.zip压缩文件里面有很多目录和子目录，并且子目录中其实都是歌曲mp3文件，我想把这些文件都下载到第一级目录，而不是一层一层建目录：

          unzip -j music.zip


## 安装软件
我用的Linux发行版本时Ubuntu，所以我基于这个版本来说。当我们在Ubuntu上安装一个软件时，常用shell来安装：

  `sudo apt-get install software`

有时候我们在这里面装不了某些软件，我们会下一些文件需自己来安装，如deb、bin、rpm以及一些压缩包。
deb类型的是Ubuntu可安装的类型。
rpm类型的是suse和Fedora上可安装的类型。
而tar.gz是所有Linux发行版本都可使用的，这是源程序的压缩包，需要我们自己编译。

  1. .deb 既然是Ubuntu上的可安装程序，最简单的方法是双击即可。当然我们也可以通过dpkg来装：

    `dpkg -i xxx.deb`

  2、.rpm 因为rpm是Fedora上的可安装程序，在Ubuntu上不能双击运行，一般的方法是我们用alien把rpm转换为deb格式的再安装。Ubuntu没有默认安装alien，所以先安装alien：

    `sudo apt-get install alien`

  然后用alien转换：sudo alien xxx.rpm
  这一步以后会生成一个同名的xxx.deb文件， 然后就可以安装了，但是这种方式不能保证100%成功。
  3. tar.gz、tar.bz2等格式的源码包，我们首先需要解压后一般自己编译：

    `./config
    make
    make install`

再说一下软件的卸载方式：
  1. APT方式：
  移除式卸载，移除软件包:

  `apt-get remove xxx`

  清除式卸载，把与软件安装有关的配置一起卸载：

  `apt-get --purge remove xxx`

  2. dpkg方式：
  移除式卸载：`dpkg -r xxx`

  清除式卸载：`dpkg -P xxx`


## 权限设置
### chown
chmod命令用来变更文件或目录的权限。

- 格式
      chmod [选项]... 模式[,模式]... 文件...
      chmod [-cfvR] [–help] [–version] mode file…
  
  mode：权限设定字符串，格式为：
        [ugoa]*([-+=]([rwxXst]*|[ugo]))+|[-+=][0-7]+
> u User，即文件或目录的拥有者；  
  g Group，即文件或目录的所属群组；  
  o Other，除了文件或目录拥有者或所属群组之外，其他用户皆属于这个范围；  
  a All，即全部的用户，包含拥有者，所属群组以及其他用户；  
  r 读取权限，数字代号为“4”;   
  w 写入权限，数字代号为“2”；  
  x 执行或切换权限，数字代号为“1”；  
  \- 不具任何权限，数字代号为“0”；
  s 特殊功能说明：变更文件或目录的权限。  

- 选项
> -c或——changes：效果类似“-v”参数，但仅回报更改的部分；   -f或--quiet或——silent：不显示错误信息；  
-R或——recursive：递归处理，将指令目录下的所有文件及子目录一并处理；  
-v或——verbose：显示指令执行过程；  
--reference=<参考文件或目录>：把指定文件或目录的所属群组全部设成和参考文件或目录的所属群组相同；  
<权限范围>+<权限设置>：开启权限范围的文件或目录的该选项权限设置；  
<权限范围>-<权限设置>：关闭权限范围的文件或目录的该选项权限设置；   
<权限范围>=<权限设置>：指定权限范围的文件或目录的该选项权限设置；  

- 参数
> 权限模式：指定文件的权限模式；  
文件：要改变权限的文件。

### chown
通过chown改变文件的拥有者和群组。在更改文件的所有者或所属群组时，可以使用用户名称和用户识别码设置。普通用户不能将自己的文件改变成其他的拥有者。其操作权限一般为管理员。

- 语法
      chgrp(选项)(参数)
      chown [选项]... [所有者][:[组]] 文件...
  
- 选项
> -c或——changes：效果类似“-v”参数，但仅回报更改的部分；   -f或--quite或——silent：不显示错误信息；  
-h或--no-dereference：只对符号连接的文件作修改，而不更改其他任何相关文件；  
-R或——recursive：递归处理，将指定目录下的所有文件及子目录一并处理；  
-v或——version：显示指令执行过程；  
--dereference：效果和“-h”参数相同；  
--help：在线帮助；  
--reference=<参考文件或目录>：把指定文件或目录的拥有者与所属群组全部设成和参考文件或目录的拥有者与所属群组相同；  
--version：显示版本信息。  

- 参数
> 用户：组：指定所有者和所属工作组。当省略“：组”，仅改变文件所有者；  
文件：指定要改变所有者和工作组的文件列表。支持多个文件和目标，支持shell通配符。  

### chgrp
chgrp命令用来改变文件或目录所属的用户组。该命令用来改变指定文件所属的用户组。其中，组名可以是用户组的id，也可以是用户组的组名。

- 语法
      chgrp(选项)(参数)
      chgrp [选项]... 用户组 文件...
    
- 选项
> -c或——changes：效果类似“-v”参数，但仅回报更改的部分；   -f或--quiet或——silent：不显示错误信息；  
-h或--no-dereference：只对符号连接的文件作修改，而不是该其他任何相关文件；  
-R或——recursive：递归处理，将指令目录下的所有文件及子目录一并处理；  
-v或——verbose：显示指令执行过程； 
--reference=<参考文件或目录>：把指定文件或目录的所属群组全部设成和参考文件或目录的所属群组相同；  

- 参数
> 组：指定新工作名称；  
文件：指定要改变所属组的文件列表。多个文件或者目录之间使用空格隔开。

### ls

### grep

### find

### cp

### mv

### rm

### ps

### kill

### killall

### file

### cat

### vim

### gcc

### time

### blkid
blkid主要用来对系统的块设备（包括交换分区）所使用的文件系统类型、LABEL、UUID等信息进行查询

  
## linux问题
- ubuntu登陆界面循环登陆，黑屏后闪退，又返回登陆界面
> 解决方法：重新安装 ubuntu-desktop unity
        sudo apt-get install --reinstall ubuntu-desktop
        sudo apt-get install --reinstall unity

参考资料:
---
[http://man.linuxde.net/](http://man.linuxde.net/)  
[http://www.cnblogs.com/chinareny2k/archive/2010/01/05/1639468.html](http://www.cnblogs.com/chinareny2k/archive/2010/01/05/1639468.html)  
[http://www.jb51.net/LINUXjishu/43356.html](http://www.jb51.net/LINUXjishu/43356.html)  
[http://ibeyond.blog.51cto.com/1988404/400067](http://ibeyond.blog.51cto.com/1988404/400067)  
