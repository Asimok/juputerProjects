---
title: emqx配置https并使用nginx反向代理
cover: https://gitee.com/Asimok/picgo/raw/master/img/MacBookPro/20201121105709.jpg
categories: 教程
tags:
  - emqx
  - MQTT
  - https
  - nginx
  - ssl
  - wss
keywords: 'emqx,MQTT,ssl,wss,https,nginx'
---

# emqx配置https并使用nginx反向代理



1. 下载域名证书，找到.crt或.key，编辑器打开，复制秘钥文本，找在线转pem工具，生成.pem文件。

![621605869569_.pic_hd](https://gitee.com/Asimok/picgo/raw/master/img/MacBookPro/20201121105009.jpg)

2. 在emq中启动ssl

![641605869642_.pic_hd](https://gitee.com/Asimok/picgo/raw/master/img/MacBookPro/20201121105109.jpg)

3. 配置nginx的反向代理

![661605869702_.pic_hd](https://gitee.com/Asimok/picgo/raw/master/img/MacBookPro/20201121105124.jpg)

- 用ngix反向代理后，wss连接端口就成了443，不是8084。

![681605869823_.pic_hd](https://gitee.com/Asimok/picgo/raw/master/img/MacBookPro/20201121105147.jpg)



![691605869841_.pic_hd](https://gitee.com/Asimok/picgo/raw/master/img/MacBookPro/20201121105159.jpg)

4. 配置文件

![711605869889_.pic_hd](https://gitee.com/Asimok/picgo/raw/master/img/MacBookPro/20201121105227.jpg)



![721605869914_.pic_hd](https://gitee.com/Asimok/picgo/raw/master/img/MacBookPro/20201121105245.jpg)



![751605870352_.pic_hd](https://gitee.com/Asimok/picgo/raw/master/img/MacBookPro/20201121105709.jpg)



- 查看安装路径

![761605870415_.pic_hd](https://gitee.com/Asimok/picgo/raw/master/img/MacBookPro/20201121105423.jpg)

> 但不知道什么原因，不使用nginx做反向代理的话，用wss 8084 是无法连接的。