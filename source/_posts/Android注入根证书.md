---
title: Andorid注入根证书
date: 2024-09-20 15:51:28
tags: 爬虫
---

## Problem

https://httptoolkit.com/blog/intercepting-android-https/#android-certificate-stores

证书转换

openssl x509 -inform der -in xxx.cer -out xxx.pem
openssl x509 -inform PEM -subject_hash_old -in xxx.pem -noout
# ???
mv xxx.pem ???.0

adb push ???.0 /sdcard

adb shell
cd /sdcard

mkdir tmp_ca
cp /system/etc/security/cacerts/* tmp_ca/
mount -t tmpfs tmpfs /system/etc/security/cacerts

cp ???.0 tmp_ca
cp tmp_ca/* /system/etc/security/cacerts/

chown root:root /system/etc/security/cacerts/*
chmod 755 /system/etc/security/cacerts
chmod 644 /system/etc/security/cacerts/*
chcon u:object_r:system_file:s0 /system/etc/security/cacerts/*