---
layout: post
title: ��ȡKeyStore SHA1��
category: Android
tags: keystore
keywords: keystore,sha1
description:
---

keystore��SHA1����ʽ�磺

    SHA1: 9D:66:68:DB:D1:96:39:3D:CA:5D:06:3D:BC:8B:87:34:99:7E:29:06

��cmd�������в�ѯSHA1�룺

1. ��cmd���������
2. ����keystore�ļ�Ŀ¼����Ĭ��debug.keystoreΪ�����ڵ�ǰ�û�Ŀ¼��.adnroidĿ¼�¡�
3. ��������

        keytool -list -v -keystore debug.keystore
    
    ������ʾ������Կ����(Ĭ��û��)�����س����õ�debug.keystore��ǩ�����顣  
    ![keystore info](/assets/images/keystore-info.jpg)