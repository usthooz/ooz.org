---
title: Golang-微信分享及接口调用所需签名获取工具
date: 2019-03-29 21:40:07
categories: [开源项目]
tags: [Golang]
---

## 概述
在进行微信Js Sdk调用时，需要首先获取到签名，通过签名进行授权以及接口调用，wxsign使用Golang编写，完成签名授权验证。

<!-- more -->

## 功能
- 获取微信分享所需要的js签名信息

- 返回签名信息结构
```
{
    Appid     string `json:"appid"`
	Noncestr  string `json:"noncestr"`
	Timestamp string `json:"timestamp"`
	Url       string `json:"url"`
	Signature string `json:"signature"`
}	
```

## 安装

```
- go get github.com/usthooz/wxsign
```

## 使用

```
package main

import (
	"fmt"

	"github.com/usthooz/wxsign"
	redis "gopkg.in/redis.v3"
)

func init() {
	// 初始化缓存access_token及ticket的redis
	rdsClient := redis.NewClient(&redis.Options{
		Addr: "127.0.0.1:6379",
	})
	wxsign.WxSignRdsInit(rdsClient)
}

func main() {
	ws := wxsign.New(
		"appid",
		"secret",
		// 缓存access_token使用的redis key
		"wxsign:token",
		// 缓存ticket使用的redis key
		"wxsign:ticket",
	)
	sign, err := ws.GetJsSign("https://www.ooz.ink")
	if err != nil {
		fmt.Print("Get js sign err-> %#v", err)
		return
	}
	fmt.Print("Js Sign: %#v", sign)
}
```