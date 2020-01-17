title: Hexo入门、安装、配置
tags:
  - Hexo
categories:
  - Hexo
description: 教你从小白开始搭建自己的博客网站，记录成长之路.....
date: 2019-09-29 15:19:00
---
#### 准备
1. 安装 __Node.js__
2. 安装 __Hexo__
---
#### 安装node.js

1. 首先下载[Node.js](https://nodejs.org/en/download/)下载完成后按照操作步骤依次进行，等待安装完成。  
**`注意:`** 在 **Windows** 上安装时务必选择全部组件，包括勾选 `Add to Path`。

2. 安装完成后，在Windows环境下，请打开命令提示符，然后输入 `node -v`，如果安装正常，会看到 `v12.10.0` 这样的输出：   
    ```
    C:\Users>node -v
    v12.10.0 ## 注意这里是依据下载版本显示
    ```
    继续在命令提示符输入`node`，此刻你将进入 `Node.js` 的交互环境。在交互环境下，你可以输入任意 `JavaScript` 语句，例如 `1+1`，回车后将得到输出结果。    
    要退出Node.js环境，连按两次Ctrl+C。   
    在Mac或Linux环境下，请打开终端，然后输入 `node -v`，你应该看到如下输出：   
    ```
    $ node -v
    v12.10.0
    ```
3. 更改npm源       
   由于默认的 **hexo** 源服务器位于国外所以需要修改为国内服务器国内源地址：
	* 淘宝__npm__镜像(__npm__安装方式)  
		* 搜索地址：`http://npm.taobao.org/`
		* registry地址：`http://registry.npm.taobao.org/`
 
    * __npm__ 安装命令:    
        `npm config set registry https://registry.npm.taobao.org`   
    * 验证安装：  
        ` npm config get registry`   
    
    如果返回 `https://registry.npm.taobao.org`，说明镜像配置成功。
---
#### 安装hexo

1.运行命令即可使用 `npm` 安装 `Hexo `
```
$ npm install -g hexo-cli 
```
2. 使用npm命令进行安装
```
$ npm install hexo
```
1. 执行 `hexo` 即可启动服务
```
hexo server
```
默认是4000端口，使用 `http://localhost:4000` 即可访问hexo页面

__ps：__ [官方文档](https://hexo.io/zh-cn/docs)