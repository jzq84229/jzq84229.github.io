---
layout: post
title: APK签名apktool及jarsigner命令
category: 笔记
tags: apktool, jarsigner
keywords:
description:
---

### keytool用法


- -certreq            生成证书请求  
用法：keytool -certreq [OPTION]...
```

  -alias <alias>                  要处理的条目的别名
  -sigalg <sigalg>                签名算法名称
  -file <filename>                输出文件名
  -keypass <arg>                  密钥口令
  -keystore <keystore>            密钥库名称
  -dname <dname>                  唯一判别名
  -storepass <arg>                密钥库口令
  -storetype <storetype>          密钥库类型
  -providername <providername>    提供方名称
  -providerclass <providerclass>  提供方类名
  -providerarg <arg>              提供方参数
  -providerpath <pathlist>        提供方类路径
  -v                              详细输出
  -protected                      通过受保护的机制的口令
```

- -changealias        更改条目的别名  
用法：keytool -changealias [OPTION]...
```
-alias <alias>                  要处理的条目的别名
-destalias <destalias>          目标别名
-keypass <arg>                  密钥口令
-keystore <keystore>            密钥库名称
-storepass <arg>                密钥库口令
-storetype <storetype>          密钥库类型
-providername <providername>    提供方名称
-providerclass <providerclass>  提供方类名
-providerarg <arg>              提供方参数
-providerpath <pathlist>        提供方类路径
-v                              详细输出
-protected                      通过受保护的机制的口令
```


- -delete             删除条目  
用法：keytool -delete [OPTION]...
```
-alias <alias>                  要处理的条目的别名
-keystore <keystore>            密钥库名称
-storepass <arg>                密钥库口令
-storetype <storetype>          密钥库类型
-providername <providername>    提供方名称
-providerclass <providerclass>  提供方类名
-providerarg <arg>              提供方参数
-providerpath <pathlist>        提供方类路径
-v                              详细输出
-protected                      通过受保护的机制的口令
```


- -exportcert         导出证书
- -genkeypair         生成密钥对
- -genseckey          生成密钥
- -gencert            根据证书请求生成证书
- -importcert         导入证书或证书链
- -importpass         导入口令
- -importkeystore     从其他密钥库导入一个或所有条目
- -keypasswd          更改条目的密钥口令
- -list               列出密钥库中的条目
- -printcert          打印证书内容
- -printcertreq       打印证书请求的内容
- -printcrl           打印 CRL 文件的内容
- -storepasswd        更改密钥库的存储口令


### jarsigner用法
用法: jarsigner [选项] jar-file 别名  
       jarsigner -verify [选项] jar-file [别名...]

```
[-keystore <url>]           密钥库位置
[-storepass <口令>]         用于密钥库完整性的口令
[-storetype <类型>]         密钥库类型
[-keypass <口令>]           私有密钥的口令 (如果不同)
[-certchain <文件>]         替代证书链文件的名称
[-sigfile <文件>]           .SF/.DSA 文件的名称
[-signedjar <文件>]         已签名的 JAR 文件的名称
[-digestalg <算法>]        摘要算法的名称
[-sigalg <算法>]           签名算法的名称
[-verify]                   验证已签名的 JAR 文件
[-verbose[:suboptions]]     签名/验证时输出详细信息。
                            子选项可以是 all, grouped 或 summary
[-certs]                    输出详细信息和验证时显示证书
[-tsa <url>]                时间戳颁发机构的位置
[-tsacert <别名>]           时间戳颁发机构的公共密钥证书
[-tsapolicyid <oid>]        时间戳颁发机构的 TSAPolicyID
[-altsigner <类>]           替代的签名机制的类名
[-altsignerpath <路径列表>] 替代的签名机制的位置
[-internalsf]               在签名块内包含 .SF 文件
[-sectionsonly]             不计算整个清单的散列
[-protected]                密钥库具有受保护验证路径
[-providerName <名称>]      提供方名称
[-providerClass <类>        加密服务提供方的名称
  [-providerArg <参数>]]... 主类文件和构造器参数
[-strict]                   将警告视为错误
```
