title: Mac笔记 (1)
date: 2013-03-19 23:45:25
tags: Mac
---

# ssh远程登录Mac OS X


## 序

近一年抱了Apple大腿之后，各种表弟、师兄、朋友陆续也开始抱Apple大腿。难免遇到各种问题，这个时候在QQ交流效率低下，简直不可忍受。也不知大腾讯啥时候支持一下QQ for Mac的远程协助。这样有时候也可以帮助一下在家的老爸老妈解决一些PC上的问题。大神们绕道无鄙视。OS X乃纯Unix血统。教科书上都写了*nix系统可以ssh登录过去搞定一切。就想着可以用ssh来解决问题。无奈网络基础确实是挂课的水平曾经尝试过一次没有成功。这次又再次遇到远程协助的问题就好好Google了一次。研究了好一会儿终于搞定，在此记录。

<!--more-->

## 参考资料

[How To Enable SSH on Your Mac](http://www.maclife.com/article/howtos/how_enable_ssh_your_mac)

## 开始打怪

### 环境

1. 一台低端D-Link的无线路由用PPoE上网方式
2. 一台低端Macbook pro安装OS X 10.8.3

### 设置你的Mac在局域网的IP为固定IP

这一步参考资料上是有的。应该也是必须的。有两种设置方法，一种是参考资料上写的那种在OS X里面设置

![图1](http://ww2.sinaimg.cn/large/bfadf3bejw1e2vjoar7hbj.jpg)

![图2](http://ww4.sinaimg.cn/large/a74ecc4cjw1e2vjoji35vj.jpg)

![图3](http://ww3.sinaimg.cn/large/a74e55b4jw1e2vjoswx7lj.jpg)


我的是直接在路由里面设置的。不同路由不一样，请自行处理

### 在路由里面设置转发端口和规则

在参考资料里面用的是Airport。给我找了一晚上设置界面没有找到。网络知识匮乏的后果阿。后来仔细阅读了一下觉得应该在自己的路由里面设置的。尝试了一下果然如此。


原图


![原图](http://www.maclife.com/files/u12635/ssh_router_setting_0.png)


我设置的真相

![设置的真相](http://ww1.sinaimg.cn/large/a74eed94jw1e2vk1nmz9wj.jpg)

注意的一点是IP地址填写的是你刚刚设置自己的固定IP地址。端口两个都是22，因为ssh用的是22端口。


### OS X上打开SSH Share

其实很简单

![设置1](http://ww3.sinaimg.cn/large/bfadf3bejw1e2vk4y5jocj.jpg)

![设置2](http://ww4.sinaimg.cn/large/a74ecc4cjw1e2vk5i19dcj.jpg)


### 打完收工

测试， 我连上我的vps 然后在vps上连接我的mac

![测试](http://ww1.sinaimg.cn/large/a74e55b4jw1e2vkau1lsxj.jpg)



## 总结

基础差害死人阿，通过这些小事情。证明了我还有很多需要学习的地方。

厦门，晚安。睡觉，又过12点了。







