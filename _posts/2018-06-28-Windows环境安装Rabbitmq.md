---
layout: master
title:  Windows环境安装Rabbitmq
description:  yuminglang.com,提供web前端、java、linux、数据库相关技术知识共享。
categories: Java
syntax: 9
---
在Windows系统上安装配置Rabbitmq.

### 1.下载Rabbitmq
1.1 Rabbitmq官网地址: https://www.rabbitmq.com

1.2 Rabbitmq官网下载页: http://www.rabbitmq.com/download.html

1.3 Windows版本的可执行安装包下载页: http://www.rabbitmq.com/install-windows.html

1.4 选择 `Installer for Windows systems (from Bintray)` 版下载

### 2.下载Erlang
2.1 Erlang官网地址: http://www.erlang.org

2.2 Erlang官网下载页: http://www.erlang.org/downloads

2.3 根据Windows系统类型选择性下载64位`OTP 21.0.1 Windows 64-bit Binary File` 或者32位 `OTP 21.0.1 Windows 32-bit Binary File`安装包

### 3.安装Erlang
3.1 选中Erlang的`opt`安装包，鼠标右键`以管理员身份运行`

3.2 将默认安装目录`C:\Program Files\erlxx.x.x`修改为不带空格的安装目录，如：`C:\erlxx.x.x`,下一步至`Install`

3.3 安装`Miccrosoft Visual C++ 2013 Redistributable`

3.4 安装完成`Close`

### 4.安装Rabbitmq
4.1 选中安装Rabbitmq的`rabbitmq-server`安装包，鼠标右键`以管理员身份运行`

4.2 将默认安装目录`C:\Program Files\RabbitMQ Server`修改为不带空格的安装目录，如：`C:\RabbitMQServer`,接着`Install`...`Finish`完成安装

### 5.环境变量配置和服务管理
5.1 配置环境变量后，可在命令行中执行RabbitMQ常用命令
系统变量新增 `MQ_HOME` 和 `ERLANG_HOME`：

变量名：`MQ_HOME`，变量值：`C:\RabbitMQServer\rabbitmq_server-3.7.6`

变量名：`ERLANG_HOME`，变量值：`C:\erl10.0.1`

系统变量`Path`中增加`%MQ_HOME%\sbin`和`%ERLANG_HOME%\bin`

特别说明：Rabbitmq的可执行命令在安装目录`sbin`目录中，Windows的开始菜单中提供服务管理工具

### 6.安装插件登陆Web管理页
1.Rabbitmq的WEB管理插件（Management Plugin）安装，正常情况下在命令行执行`rabbitmq-plugins enable rabbitmq_management`会有如下输出，表示Rabbitmq和Erlang依赖安装成功：
```sh
C:\Users\Gaxr>rabbitmq-plugins enable rabbitmq_management
Enabling plugins on node rabbit@DESKTOP-FCEHRMP:
rabbitmq_management
The following plugins have been configured:
  rabbitmq_management
  rabbitmq_management_agent
  rabbitmq_web_dispatch
Applying plugin configuration to rabbit@DESKTOP-FCEHRMP...
The following plugins have been enabled:
  rabbitmq_management
  rabbitmq_management_agent
  rabbitmq_web_dispatch

set 3 plugins.
Offline change; changes will take effect at broker restart.
```
2. Management Plugin安装失败，原因是Erlang的版本太新，可以下载稍旧版本Erlang的opt软件安装.

Erlang和Rabbitmq版本不匹配时执行`rabbitmqctl start`这个不存在的命令会返回错误提示：
```sh
C:\Users\Gaxr>rabbitmqctl start
=SUPERVISOR REPORT==== 28-Jun-2018::12:50:19.115000 ===
    supervisor: {local,'Elixir.Logger.Supervisor'}
    errorContext: start_error
    reason: noproc
    offender: [{pid,undefined},
               {id,'Elixir.Logger.ErrorHandler'},
               {mfargs,
                   {'Elixir.Logger.Watcher',start_link,
                       [{error_logger,'Elixir.Logger.ErrorHandler',
                            {true,false,500}}]}},
               {restart_type,permanent},
               {shutdown,5000},
               {child_type,worker}]
Could not start application logger: Logger.App.start(:normal, []) returned an error: shutdown: failed to start child: Logger.ErrorHandler
    ** (EXIT) no process: the process is not alive or there's no process currently associated with the given name, possibly because its application isn't started
```

Erlang和Rabbitmq版本匹配可用时执行`rabbitmqctl start`这个不存在的命令会返回以下提示：
```sh
C:\Users\Gaxr>rabbitmqctl start

Command 'start' not found.
Did you mean 'stop'?
```

3.打开`http://localhost:15672`登陆页，默认用户名：`guest`，默认登陆密码：`guest` 

