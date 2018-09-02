---
layout: post
title:  "PHP多进程、多线程、swoole、redis安装"
date:   2018-08-07 14:08:53
categories: 技术
tags: swoole redis pcntl pthreads php 多进程 多线程
excerpt: 
---

* content
{:toc}

## 项目需要用到的扩展
* `pcntl` 多进程(PHP自带，只需要编译的时候添加就可以了)
* `thread` 多线程
* `swoole` 网络编程
* `redis` 缓存
* `curl` curl网络请求


## 参考文档
* [http://php.net/manual/zh/pthreads.installation.php](http://php.net/manual/zh/pthreads.installation.php)
* [https://wiki.swoole.com/wiki/page/6.html](https://wiki.swoole.com/wiki/page/6.html)
* [https://github.com/krakjoe/pthreads](https://github.com/krakjoe/pthreads)
* [https://www.cnblogs.com/tinywan/p/7230039.html](https://www.cnblogs.com/tinywan/p/7230039.html)
* [https://blog.csdn.net/chenmoimg_/article/details/79712236](https://blog.csdn.net/chenmoimg_/article/details/79712236)





### 下载所有需要的文件
* [php-7.2.8.tar.gz](http://hk2.php.net/distributions/php-7.2.8.tar.gz)
* [phpredis-3.1.4.tar.gz](https://codeload.github.com/phpredis/phpredis/tar.gz/3.1.4)
* [swoole-src-4.0.3.tar.gz](https://codeload.github.com/swoole/swoole-src/tar.gz/v4.0.3)
* [pthreads](https://github.com/krakjoe/pthreads) 在这个地方下载的才有用  master分支
* [libcurl](https://curl.haxx.se/download/curl-7.55.1.tar.gz)

## 安装步骤
### 安装php7并开启多进程
[php7.2.8下载地址](http://hk2.php.net/distributions/php-7.2.8.tar.gz)

```shell
# 解压文件
tar zxvf php-7.2.8.tar.gz
cd php-7.2.8

# 配置php的安装路径以及开启线程非安全模式开启多线程安装mysqli
./configure --prefix=/home/php --with-config-file-path=/home/php --enable-maintainer-zts --enable-pcntl --enable-pdo --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd

# 编译 安装
make 
sudo make install

# 安装成功提示
# Installing shared extensions:     /home/php/lib/php/extensions/no-debug-zts-20170718/
# Installing PHP CLI binary:        /home/php/bin/
# Installing PHP CLI man page:      /home/php/php/man/man1/
# Installing phpdbg binary:         /home/php/bin/
# Installing phpdbg man page:       /home/php/php/man/man1/
# Installing PHP CGI binary:        /home/php/bin/
# Installing PHP CGI man page:      /home/php/php/man/man1/
# Installing build environment:     /home/php/lib/php/build/
# Installing header files:          /home/php/include/php/
# Installing helper programs:       /home/php/bin/
#   program: phpize
#   program: php-config
# Installing man pages:             /home/php/php/man/man1/
#   page: phpize.1
#   page: php-config.1
# Installing PEAR environment:      /home/php/lib/php/
# [PEAR] Archive_Tar    - installed: 1.4.3
# [PEAR] Console_Getopt - installed: 1.4.1
# [PEAR] Structures_Graph- installed: 1.1.1
# [PEAR] XML_Util       - installed: 1.4.2
# [PEAR] PEAR           - installed: 1.10.5
# Warning! a PEAR user config file already exists from a previous PEAR installation at '/root/.pearrc'. You may probably want to remove it.
# Wrote PEAR system config file at: /home/php/etc/pear.conf
# You may want to add: /home/php/lib/php to your php.ini include_path
# /root/桌面/php/anzhuangbao/php-7.2.8/build/shtool install -c ext/phar/phar.phar /home/php/bin
# ln -s -f phar.phar /home/php/bin/phar
# Installing PDO headers:           /home/php/include/php/ext/pdo/

# 讲php.ini拷贝到安装目录下面去
cp php.ini-development /home/php/php.ini
```
安装完成



### 安装pthreads多线程
首先需要去下载`pthreads`。我是在[github-pthreads](https://github.com/krakjoe/pthreads)这里下载。master分支上。安装过程没有出现问题。安装之后查看版本是3.1.7版本的。

1. 下载github上面的文件。
2. 进入目录

```shell
cd pthreads-master
/home/php/bin/phpize
./configure --with-php-config=/home/php/bin/php-config
make
sudo make install

# 执行到这里会出现这样的内容
# Installing shared extensions:     /home/php/lib/php/extensions/no-debug-zts-20170718/
# 表示已经安装顺利的安装完成了。

# 将刚才php安装目录下面的php.ini-development拷贝到php安装目录下面去
cp php-7.2.8/php.ini-development /home/php/php.ini

# 在php.ini里面加入这个扩展
echo "extension=pthreads.so" >> /home/php/php.ini

# 可以查看是否安装成功
/home/php/bin/php -m
```
安装完成


### 安装swoole
[下载地址](https://codeload.github.com/swoole/swoole-src/tar.gz/v4.0.3)

```shell
# 解压文件
tar zxvf swoole-src-4.0.3.tar.gz

# 进入目录
cd swoole-src-4.0.3

# 使用安装php的目录下的phpize生成编译脚本
/home/php/bin/phpize 

# 配置编译脚本
./configure --with-php-config=/home/php/bin/php-config

# 编译
make

# 安装
sudo make install

# 屏幕安装成功提示输出
# Installing shared extensions:     /home/php/lib/php/extensions/no-debug-zts-20170718/
# Installing header files:          /home/php/include/php/


# 在php.ini里面加入这个扩展
echo "extension=swoole.so" >> /home/php/php.ini

# 可以查看是否安装成功
/home/php/bin/php -m
```

### 安装redis
[下载地址](https://codeload.github.com/phpredis/phpredis/tar.gz/3.1.4)

```shell
# 解压文件
tar zxvf phpredis-3.1.4.tar.gz 

# 进入目录
cd phpredis-3.1.4

# 使用安装php的目录下的phpize生成编译脚本
/home/php/bin/phpize 

# 配置编译脚本
./configure --with-php-config=/home/php/bin/php-config

# 编译
make

# 安装
sudo make install

# 屏幕安装成功提示输出
# Installing shared extensions:     /home/php/lib/php/extensions/no-debug-zts-20170718/


# 在php.ini里面加入这个扩展
echo "extension=redis.so" >> /home/php/php.ini

# 可以查看是否安装成功
/home/php/bin/php -m
```

### 安装curl
[libcurl下载地址](https://curl.haxx.se/download/curl-7.55.1.tar.gz)
```shell
# 下载libcurl文件
wget https://curl.haxx.se/download/curl-7.55.1.tar.gz

# 安装libcurl
tar zxvf curl-7.55.1.tar.gz 
cd curl-7.55.1
mkdir /usr/local/lib/curl
./configure --prefix=/usr/local/lib/curl/
make 
make install
# libcurl已经成功安装了
# 现在去安装phpcurl的扩展

# 进入php源码目录
cd php-7.2.8/ext/curl

# 使用安装php的目录下的phpize生成编译脚本
/home/php/bin/phpize 

# 配置编译脚本
./configure --with-php-config=/home/php/bin/php-config --with-curl=/usr/local/lib/curl/

# 编译
make

# 安装
sudo make install

# 屏幕安装成功提示输出
# Installing shared extensions:     /home/php/lib/php/extensions/no-debug-zts-20170718/

# 可以看看这个目录下面是不是已经有了curl.so
ll /home/php/lib/php/extensions/no-debug-zts-20170718/

# 在php.ini里面加入这个扩展
echo "extension=curl.so" >> /home/php/php.ini

# 可以查看是否安装成功
/home/php/bin/php -m
```


### 踩到的坑
1. mysqli扩展

因为我一开始编译php的时候没有启用mysqli。我以为后来通过添加扩展的方式安装上去就可以了。其实没有那么简单，这个mysqli是PHP内核，如果要用，在编译php的时候就要开启它。(按道理来说应该默认开启才对啊)。我搞了一晚上，还是把它安装不上去。最后无赖的选择了重新编译php启用mysqli。ojbk了。没毛病