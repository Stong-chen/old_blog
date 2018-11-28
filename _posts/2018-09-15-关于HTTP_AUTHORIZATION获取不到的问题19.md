---
layout: post
title:  "关于HTTP_AUTHORIZATION获取不到的问题"
date:   2018-09-15 10:44:04
categories: 笔记
tags: 踩坑 php apache
excerpt: 
---

* content
{:toc}

在做接口的时候。由于特发性情况，换了一台服务器。但是这台服务器用的是Apache配置的网站。原来我们的网站是放在Nginx上面的。然后就遇到了这个错误。





### 发现错误
做接口认证的时候。我们把`Authorization`放在`header`里面的。直接在php端使用`$_SERVER['HTTP_AUTHORIZATION']`变量就可以获取到`Authorization`的值。

但是不知道为什么突然获取不到了。我排查了很久。我将`Authorization`改成`Authorization1`都可以获取到。但是为什么就是`Authorization`获取不到嘞？这很奇怪。

### 错误原因
这个错误是因为我们采用了`Apache`的服务所导致的。在`Nginx`下面是没有问题的。它可以帮我们解析到`$_SERVER`变量里面去。但是`Apache`就不会了。

我们需要这样修改
如果开启了`rewrite_module`模块。

```
#Authorization Headers
RewriteCond %{HTTP:Authorization} ^(.+)$
RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
```

如果没有开启`rewrite_module`模块

```
Options +FollowSymlinks -Multiviews
RewriteEngine On
#Authorization Headers
RewriteCond %{HTTP:Authorization} ^(.+)$
RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
```

### 参考文章
* [https://www.kancloud.cn/iwzh/wzhquestion/132005](https://www.kancloud.cn/iwzh/wzhquestion/132005)