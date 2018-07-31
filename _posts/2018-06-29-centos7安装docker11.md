---
layout: post
title:  "centos7安装docker"
date:   2018-06-29 10:24:19
categories: 技术
tags: linux 技术
excerpt: 
---

* content
{:toc}

原来尝试过在ubuntu16.04上面安装docker。但是最近有个新的想法，需要在centos7上面安装docker。随便就写个教程下来。





## 官方参考网址
[docker安装方法](https://docs.docker.com/install/linux/docker-ce/centos/)

[docker-compose安装方法](https://docs.docker.com/compose/install/)

## docker安装流程
* 卸载老版本的docker

因为不太清楚你的系统是否安装了docker。所以为了保证了正确的安装。就先把它卸载了。就算你没有。执行这个语句也不会出错的。只会提示你没有docker 


```shell
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine
```

* 安装一些必须要的包

```sh
$ sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```

* 指定docker仓库地址

```
$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

* [可选]启用或者禁用一些docker的边缘仓库

```sh
#enable 启用边缘仓库
$ sudo yum-config-manager --enable docker-ce-edge
$ sudo yum-config-manager --enable docker-ce-test


#disable 禁用边缘仓库
$ sudo yum-config-manager --disable docker-ce-edge
```

* 安装

安装的时候问你的那些[y/n]全部选y就可以了

```sh
$ sudo yum install docker-ce
```


如果没问题。就已经成功安装docker了执行`docker`你会看到docker的提示框。

但是现在还没有启动docker需要执行语句`sudo systemctl start docker`来启动docker。就可以正常使用了

## 安装docker-compose

* 下载文件

```sh
sudo curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
``` 

* 修改权限

```sh
sudo chmod +x /usr/local/bin/docker-compose
```
