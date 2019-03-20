---
title: Nginx代理获取用户真实IP
date: 2019-03-20 13:49:17
categories: [运维]
tags: [Nginx]
---

### 概述
在后端开发中，经常需要获取客户端的真实IP，再使用Nginx反向代理的过程中，获取到的IP往往不是用户的真实IP，本文主要解决该问题。

<!-- more -->

### Nginx配置
```
location / {
    proxy_set_header            Host $host;
    proxy_set_header            X-real-ip $remote_addr;
    proxy_set_header            X-Forwarded-For $proxy_add_x_forwarded_for;
}
```
如上面配置，接口需要使用的时候获取X-real-ip就可以，但是经过测试以后，发现X-real-ip并不是真实的用户IP，而是Nginx代理服务器的IP，原因就是经过多级代理，$remote_addr是上一级的IP。
如果配置只有以及代理，那么获取到的IP就是准确的用户IP，但是这种情况除了在测试环境应该很少会用到。

- 名词解释
    - $remote_addr
    获取到上一级代理的IP
    - proxy_add_x_forwarded_for
    获取到结果例如：(223.104.6.125, 10.10.10.45)，第一个是用户的真实IP，第二个是一级代理的IP，依此类推。

### 解决方案
通过上面的分析我们可以从proxy_add_x_forwarded_for中获取到用户的真实IP，使用正则匹配获取第一个即可，如下：
```
location / {
    proxy_set_header            Host $host;
    set $Real $proxy_add_x_forwarded_for;
    if ( $Real ~ (\d+)\.(\d+)\.(\d+)\.(\d+),(.*) ){
        set $Real $1.$2.$3.$4;
    }
    proxy_set_header            X-real-ip $Real;
    proxy_set_header            X-Forwarded-For $proxy_add_x_forwarded_for;
}
```