# Senparc.Weixin SDK 教程

> [时间：2016.08.15] [作者：@JeffreySu] [关键字：OAuth,OpenId]

Senparc.Weixin.MP SDK从一开始就坚持开源的状态，这个过程中得到了许多朋友的认可和支持。

目前SDK已经达到比较稳定的版本，这个过程中我觉得有必要整理一些思路和经验，和大家一起分享。也欢迎大家的补充！

1、开源项目：https://github.com/JeffreySu/WeiXinMPSDK

2、微信技术交流社区：http://weixin.senparc.com/QA

3、chm帮助文档下载：http://sdk.weixin.senparc.com/Document

4、Senparc.Weixin SDK官网地址：http://weixin.senparc.com

5、Senparc.Weixin.MP.Sample Demo地址：http://sdk.weixin.senparc.com

6、更多资源：https://github.com/JeffreySu/WeixinResource



Senparc.Weixin.MP SDK的目标是探索微信公众平台更好的SDK模式，并提供C#上最好的公众平台SDK开发体验。

 

索引

1. [Senparc.Weixin.MP SDK 微信公众平台开发教程（一）：微信公众平台注册](http://www.cnblogs.com/szw/archive/2013/05/20/3089479.html)
1. [Senparc.Weixin.MP SDK 微信公众平台开发教程（二）：成为开发者](http://www.cnblogs.com/szw/archive/2013/05/27/3100713.html)
1. [Senparc.Weixin.MP SDK 微信公众平台开发教程（三）：微信公众平台开发验证](http://www.cnblogs.com/szw/p/3202857.html)
1. [Senparc.Weixin.MP SDK 微信公众平台开发教程（四）：Hello World](http://www.cnblogs.com/szw/p/3202968.html)
1. [Senparc.Weixin.MP SDK 微信公众平台开发教程（五）：使用Senparc.Weixin.MP SDK](http://www.cnblogs.com/szw/p/3414732.html)
1. [Senparc.Weixin.MP SDK 微信公众平台开发教程（六）：了解MessageHandler](http://www.cnblogs.com/szw/p/3414862.html)
1. [Senparc.Weixin.MP SDK 微信公众平台开发教程（七）：解决用户上下文（Session）问题](http://www.cnblogs.com/szw/p/3414887.html)
1. [Senparc.Weixin.MP SDK 微信公众平台开发教程（八）：通用接口说明](http://www.cnblogs.com/szw/p/3750381.html)
1. [Senparc.Weixin.MP SDK 微信公众平台开发教程（九）：自定义菜单接口说明](http://www.cnblogs.com/szw/p/3750517.html)
1. [Senparc.Weixin.MP SDK 微信公众平台开发教程（十）：多客服接口说明](http://www.cnblogs.com/szw/p/3764248.html)
1. [Senparc.Weixin.MP SDK 微信公众平台开发教程（十一）：高级接口说明](http://www.cnblogs.com/szw/p/3764267.html)
1. [Senparc.Weixin.MP SDK 微信公众平台开发教程（十二）：OAuth2.0说明](http://www.cnblogs.com/szw/p/3764275.html)
1. [Senparc.Weixin.MP SDK 微信公众平台开发教程（十三）：地图相关接口说明](http://www.cnblogs.com/szw/p/4138442.html)
1. [Senparc.Weixin.MP SDK 微信公众平台开发教程（十四）：请求消息去重](http://www.cnblogs.com/szw/p/4138516.html)
1. [Senparc.Weixin.MP SDK 微信公众平台开发教程（十五）：消息加密](http://www.cnblogs.com/szw/p/4140069.html)
1. [Senparc.Weixin.MP SDK 微信公众平台开发教程（十六）：AccessToken自动管理机制](http://www.cnblogs.com/szw/p/4624149.html)
1. [Senparc.Weixin.MP SDK 微信公众平台开发教程（十七）：个性化菜单接口说明](http://www.cnblogs.com/szw/p/weixin-conditional-menu.html)
1. [Senparc.Weixin.MP SDK 微信公众平台开发教程（十八）：Web代理功能](http://www.cnblogs.com/szw/p/weixin-sdk-request-proxy.html)

## 参考资料
《微信公众账号 Senparc.Weixin.MP SDK 开发教程 索引》
http://www.cnblogs.com/szw/archive/2013/05/14/weixin-course-index.html