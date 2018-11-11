---
title: Golang配置文件解析-oozgconf
date: 2018-11-11 09:47:17
categories: [Golang]
tags: [开源项目]
---

### 概述
oozgconf是一个基于Golang开发的轻量配置文件加载工具，目前支持json、toml、yaml、xml等格式。

<!-- more -->
### 功能
- 配置文件读取
- 配置文件解析

### 支持配置文件格式
- .json
- .toml
- .xml
- .yaml

### 安装
```
$ go get -u github.com/usthooz/oozgconf
```

### 实现思路
在后端项目中，配置文件已经是一个不可或缺的东西了，格式也是多种多样。

#### 流程结构
如下图所示为项目实现流程及结构：
![在这里插入图片描述](design.png)

#### 代码目录结构
![在这里插入图片描述](tree.png)

#### 主要代码
- 配置文件后缀名常量定义
```
const (
    JsonSub string = "json"
    YamlSub string = "yaml"
    TomlSub string = "toml"
    XmlSub  string = "xml"
)
```

- 对象结构
```
type OozGconf struct {
    // ConfPath config file path->default: ./config/config.yaml
    ConfPath string
    // Subffix config file subffix
    Subffix string
}
```

- 新建gconf对象
在使用时，如果不指定配置文件的路径，那么默认为./config/config.yaml，同时如果不指定文件类型，则自动通过解析文件名来获得配置文件的后缀。
```
// NewConf new conf object
func NewConf(confParam *OozGconf) *OozGconf {
    if len(confParam.ConfPath) == 0 {
        confParam.ConfPath = "./config/config.yaml"
    }
    return confParam
}
```

- 获取配置
```
/*
    confpath: config file path->default: ./config/config.yaml
    subffix: config file subffie->option
*/
func (oozConf *OozGconf) GetConf(conf interface{}) error {
    // read config file
    bs, err := ioutil.ReadFile(oozConf.ConfPath)
    if err != nil {
        return err
    }
    if len(oozConf.Subffix) == 0 {
        // get file subffix
        oozConf.Subffix = strings.Trim(path.Ext(path.Base(oozConf.ConfPath)), ".")
    }
    // check analy
    switch oozConf.Subffix {
    case JsonSub:
        err = json.Unmarshal(bs, conf)
    case TomlSub:
        err = toml.Unmarshal(bs, conf)
    case YamlSub:
        err = yaml.Unmarshal(bs, conf)
    case XmlSub:
        err = xml.Unmarshal(bs, conf)
    default:
        err = fmt.Errorf("GetConf: non support this file type...")
    }
    return err
}
```

### 使用例程
- example
```
import (
    "github.com/usthooz/oozgconf"
    "github.com/usthooz/oozlog/go"
)
type Config struct {
    Author string
    Mysql  struct {
        User     string
        Password string
    }
}
func main() {
    var (
        conf Config
    )
    // new conf object
    ozconf := oozgconf.NewConf(&oozgconf.OozGconf{
        ConfPath: "./config.json", // 可选，默认为./config/config.yaml
        Subffix:  "", // 可选，如果不指定则自动解析文件名获取
    })
    // get config
    err := ozconf.GetConf(&conf)
    if err != nil {
        uoozg.Errorf("GetConf Err: %v", err.Error())
    }
    uoozg.Infof("Res: %v", conf)
}
```

#### 运行结果
![在这里插入图片描述](result.png)
