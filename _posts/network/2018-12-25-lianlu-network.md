---
layout: post
title: '交接双链路配置'
tags: [network]
---
* [1.交接双链路配置](#1)

* * [1.1华三模拟器介绍](#1.1)

* * [1.2增加两个交换机](#1.2)

* * [1.3配置第二台交换机](#1.3)

* * [1.4添加一台pc机](#1.4)

* * [1.5测试PC3](#1.5)

* * [1.6测试PC4](#1.6)

* * [1.7试验整体拓补图](#1.7)

<h1 id="1">1.交接机双链路配置</h1>

<h2 id="1.1">1.1华三模拟器介绍</h2>
打开模拟器如下图：
![](/images/network/shuanglianlu1-2019-2-6.png)
<h2 id="1.2">1.2增加两个交换机</h2>
项目增加两个交换机做静态链路聚合,定义设备名第一台：定义设备名为ailab1。
![](/images/network/shuanglianlu2-2019-2-6.png)
启动命令终端，输入Ctrl+d
![](/images/network/shuanglianlu3-2019-2-6.png)
进入系统视图模式
![](/images/network/shuanglianlu4-2019-2-6.png)
更改名字为ailab1
![](/images/network/shuanglianlu5-2019-2-6.png)
创建二层聚合端口
![](/images/network/shuanglianlu6-2019-2-6.png)
分别将设备A上端口1/0/1,1/0/2,1/0/3加入到聚合组中
![](/images/network/shuanglianlu7-2019-2-6.png)
![](/images/network/shuanglianlu8-2019-2-6.png)
<h2 id="1.3">1.3配置第二台交换机</h2>
修改名为ailab2
![](/images/network/shuanglianlu9-2019-2-6.png)
创建二层聚合端口
![](/images/network/shuanglianlu10-2019-2-6.png)
分别将设备A上端口1/0/1,1/0/2,1/0/3加入到聚合组中
![](/images/network/shuanglianlu11-2019-2-6.png)
![](/images/network/shuanglianlu12-2019-2-6.png)
查看配置是否成功
![](/images/network/shuanglianlu13-2019-2-6.png)
![](/images/network/shuanglianlu14-2019-2-6.png)
进入vlan10
![](/images/network/shuanglianlu15-2019-2-6.png)
![](/images/network/shuanglianlu16-2019-2-6.png)
<h2 id="1.4">1.4添加一台pc机</h2>
![](/images/network/shuanglianlu17-2019-2-6.png)
![](/images/network/shuanglianlu18-2019-2-6.png)
![](/images/network/shuanglianlu19-2019-2-6.png)
 测试是否可以ping通
![](/images/network/shuanglianlu20-2019-2-6.png)
![](/images/network/shuanglianlu21-2019-2-6.png)
配置第二台交换机ip
![](/images/network/shuanglianlu22-2019-2-6.png)
![](/images/network/shuanglianlu23-2019-2-6.png)
![](/images/network/shuanglianlu24-2019-2-6.png)
![](/images/network/shuanglianlu25-2019-2-6.png)
![](/images/network/shuanglianlu26-2019-2-6.png)
<h2 id="1.5">1.5测试PC3</h2>
![](/images/network/shuanglianlu27-2019-2-6.png)
<h2 id="1.6">1.6测试PC4</h2>
![](/images/network/shuanglianlu28-2019-2-6.png)
<h2 id="1.7">1.7试验整体拓补图</h2>
![](/images/network/shuanglianlu29-2019-2-6.png)
