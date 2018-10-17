---
title: Node.js之Mysql基本操作
date: 2018-10-17 19:34:20
categories: [Node.js]
tags: [基础]
---

###  依赖安装
首先执行npm命令安装Mysql依赖，如下：
```
npm i -d mysql
```
<!-- more -->
如下图所示安装完成：
![在这里插入图片描述](https://img-blog.csdn.net/20181017174456785?water![在这里插入图片描述](https://img-blog.csdn.net/20181017174458551?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4Nzk2MzQ1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)mark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4Nzk2MzQ1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

### 数据库连接
```
// 加载mysql模块
var mysql = require('mysql');
// 创建数据库连接
var conn = mysql.createConnection({
    host: 'localhost',
    user: 'root',
    password: '0707',
    database: 'nodetest',
});
conn.connect();
// 测试一下
conn.query('select count(1) from user', function (error, results) {
    if (error){
        console.log(error);
        return;
    };
    console.log('results', results);
});
// output: results [ RowDataPacket { 'count(1)': 1 } ]
```

### 基本操作
包括基本的增删改查操作。
对于数据查询其实只需要关注query即可，如下：

```
conn.query(sql,params,function (err,results) {
});
```
sql为需要执行的插入、查询、更新、删除等的数据库sql语句，而params是通配符?所对应的值，其中err为操作过程中遇到的错误，而results则是sql实行的结果。

#### 插入
```
// insert
var insertSql = 'insert into user(name) values(?)'
var insertParams = ['node']
conn.query(insertSql, insertParams, function (err, results) {
    if (err) {
        console.log('[INSERT ERROR] - ', err);
        return;
    };
    console.log('---INSERT---');
    console.log(results);
    console.log('---END---');
});
```
执行结果如图：
![在这里插入图片描述](https://img-blog.csdn.net/20181017191726878?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4Nzk2MzQ1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

#### 查询
```
// select
var sql = 'select *from user'
conn.query(sql,function (err,results) {
    if (err){
        console.log('[SELECT ERROR] - ', err);
        return;
    };
    console.log('---SELECT---');
    console.log(results);
    console.log('---END---');
});
```
执行结果如图：
![在这里插入图片描述](https://img-blog.csdn.net/2018101719185346?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4Nzk2MzQ1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

#### 更新
```
// update
var updateSql = 'update user set name=? where id=?';
var updateParams = ['nodetest',3];
conn.query(updateSql,updateParams,function (err,results) {
    if (err) {
        console.log('[UPDATE ERROR] - ', err.message);
        return;
    }
    console.log('---UPDATE---');
    console.log('UPDATE affectedRows', results.affectedRows);
    console.log('------\n\n');
});
```
执行结果如图：
![在这里插入图片描述](https://img-blog.csdn.net/2018101719191699?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4Nzk2MzQ1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

#### 删除
```
// delete
var deleteSql = 'delete from user where id=?';
var deleteParams = [4];
conn.query(deleteSql,deleteParams,function (err,results) {
    if (err) {
        console.log('[DELETE ERROR] - ', err.message);
        return;
    }
    console.log('---DELETE---');
    console.log('DELETE affectedRows', results.affectedRows);
    console.log('------\n\n');  
});
```
执行结果如图：
![在这里插入图片描述](https://img-blog.csdn.net/20181017191931575?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4Nzk2MzQ1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

### 完整代码
[点击查看](https://github.com/usthooz/program_lan/blob/master/js/mysql/mysql.js)