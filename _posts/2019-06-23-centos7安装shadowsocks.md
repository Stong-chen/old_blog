---
layout: post
title:  "centos7安装shadowsocks"
date:   2019-06-23 16:24:00
categories: 技术
tags: centos7 shadowsocks

---
* content
{:toc}

为了能够科学上网，我只好买国外的服务器来科学上网了。前几天我的ip被封了，真是让人很蛋疼，不能特学上网了。我发现我的ipv6地址没有被封。于是就重新配置了ipv6的翻墙。记录一下





### 步骤
1. 安装pip
2. 通过pip安装shadowsocks
3. 添加配置文件
4. 启动shadowsocks

### 安装pip
```
yum -y install python-pip
# 更新pip
pip install --upgrade pip
```

### 通过pip安装shadowsocks
```
pip install shadowsocks
```

### 添加配置文件
```
vim /etc/shadowsocks.json
```

```
{
    "server" : "::",
    "server_port" : 9999,
    "local_address": "127.0.0.1",
    "local_port" : 1080,
    "password" : "password",
    "timeout" : 600,
    "method" : "rc4-md5"
}
```
server 是服务器的监听地址。这里监听了ipv6的所以地址
server_port 服务器的监听端口
local_address 本地转发的ip地址
local_port  本地转发的端口
password   ss的密码
timeout 超时时间
method  加密方式


### 启动shadowsocks
```
# 启动
ssserver -c /etc/shadowsocks.json -d start
# 停止
ssserver -c /etc/shadowsocks.json -d stop
```
