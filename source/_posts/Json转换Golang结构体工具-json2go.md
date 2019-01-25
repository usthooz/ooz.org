---
title: Json转换Golang结构体工具-json2go
date: 2019-01-25 09:57:33
categories: [Golang]
tags: [开源项目]
---

### 概述
json2go用于将json结构转换为golang使用的结构体，配置json文件，通过命令可以将转换后的结构体输出到屏幕或者输出到文件。

<!-- more -->

### 功能
- 通过读取json文件生成Golang对应的结构体
- 可选输出方式为屏幕输出以及写入到文件

### 使用

#### 安装
```
go get github.com/usthooz/json2go
cd $GOPATH/github.com/usthooz/json2go
go install
```

#### 使用方法
- 帮助信息
![帮助](1.png)

- 新建json文件
- 使用命令将json文件转换为Golang结构体，可选择输出到文件以及屏幕
- 使用默认配置

```
json2go gen_types
```
- 输出到文件

```
json2go gen_types -out_type=file -out_file=out_types.go
```

- 输出到屏幕

```
json2go gen_types -out_type=print
```