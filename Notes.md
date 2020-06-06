#  Dgraph Notes

组件名称：Dgraph
安装文档：https://dgraph.io/downloads#download-linux  
配置文档:  
支持平台：Debian家族 | RHEL家族 | Windows |macOS |source |k8s |docker

责任人：zxc

## 概要

Dgraph是一个开放源代码，可扩展，分布式，高可用性和快速的图形数据库，从头开始设计以在生产中运行。

## 环境要求

* 程序语言：   Go
* 应用服务器： apche 2.0
* 数据库：dgraph
* 依赖组件：dgraph-ratel   dgraph zero
* 其他：

## 安装说明

本项目采用源码安装方式,直接从官网下载二进制文件。

下面基于不同的安装平台，分别进行安装说明。

### CentOS  

```shell
#安装dgraph,自动同意社区许可,自动创建服务单元文件 
curl https://get.dgraph.io -sSf | bash -s -- --systemd -y  --accept-license

#服务开机自启
systemctl enable dgraph-alpha.service dgraph-ui.service dgraph-zero.service 
```

### Ubuntu (与centos相同)

```shell

```

## 路径

* 程序路径：/usr/local/bin/dgraph
* 日志路径： /var/log/dgraph
* 配置文件路径：
* 其他...

##配置

无

## 账号密码


### 数据库密码

如果有数据库  

* 数据库安装方式：
* 账号密码：

### 后台账号

如果有后台账号

* 登录地址  http://Your ip:8000
* 账号密码   
* 密码修改方案：最好是有命令行修改密码的方案

## 服务

本项目安装后自动生成：

备注：本项目安装后自动生成服务,无需自行编写服务

服务文件位置：/etc/systemd/system/

```
#   dgraph-alpha.service:
[Unit]
Description=dgraph.io Alpha instance
Wants=network.target
After=network.target dgraph-zero.service
Requires=dgraph-zero.service

[Service]
Type=simple
WorkingDirectory=/var/lib/dgraph
ExecStart=/usr/bin/bash -c 'dgraph alpha --lru_mb 2048 -p /var/lib/dgraph/p -w /var/lib/dgraph/w'
Restart=on-failure
StandardOutput=journal
StandardError=journal
User=dgraph
Group=dgraph

[Install]
WantedBy=multi-user.target


#   dgraph-zero.service:

[Unit]
Description=dgraph.io Zero instance
Wants=network.target
After=network.target

[Service]
Type=simple
WorkingDirectory=/var/lib/dgraph
ExecStart=/usr/bin/bash -c 'dgraph zero --wal /var/lib/dgraph/zw'
Restart=on-failure
StandardOutput=journal
StandardError=journal
User=dgraph
Group=dgraph

[Install]
WantedBy=multi-user.target
RequiredBy=dgraph-alpha.service


#  dgraph-ui.service:

[Unit]
Description=dgraph.io Web UI
Wants=network.target
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/bash -c 'dgraph-ratel'
Restart=on-failure
StandardOutput=journal
StandardError=journal
User=dgraph
Group=dgraph

[Install]
WantedBy=multi-user.target

```

## 环境变量

ACCEPT_LICENSE：自动同意Dgraph社区许可的条款（默认值：“ n”）。
INSTALL_IN_SYSTEMD：自动将Dgraph的安装创建为Systemd服务（默认值：“ n”）。
VERSION：手动选择Dgraph的版本（默认值：最新的稳定版本）。

## 版本号

通过如下的命令获取主要组件的版本号: 

```
# Check dgraph version
 /usr/local/bin/dgraph version
```

## 常见问题

#### 有没有管理控制台？

*http://Your ip:8000*即可访问控制台

#### 本项目需要开启哪些端口？

| item   | port |
| ------ | ---- |
| dgraph | 8080 |
| dgraph | 9080 |
| dgraph | 5080 |
| dgraph | 6080 |
| dgraph | 8000 |
| dgraph | 7080 |

#### 有没有CLI工具？

无

## 日志

* 2020-05-18完成CentOS安装研究