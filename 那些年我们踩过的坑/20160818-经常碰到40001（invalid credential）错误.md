# 经常碰到40001（invalid credential）错误

> [时间：2016.08.18] [作者：@JeffreySu] [关键字：40001,invalid credential,AccessToken]

首先我们这里确定的一个背景是AppId和Secret是正确的，在一个站点程序生命周期内已经拿到过AccessToken，
但在系统约定的过期时间内（默认为7200秒）提前过期，再次调用高级接口时返回40001的错误（invalid credential）。

这种情况下下面检查几个比较容易忽略的问题：

1. 是否存在开发（测试）环境和生产环境使用同一套账号（AppId和Secret）的情况，导致开发和测试的时候无意中刷新了Token。

    (有关微信公众号和企业号的AccessToken不同的刷新规则请见：[AccessToken的刷新规则在公众号、企业号中是不一样的](https://github.com/JeffreySu/WeixinResource/blob/master/%E9%82%A3%E4%BA%9B%E5%B9%B4%E6%88%91%E4%BB%AC%E8%B8%A9%E8%BF%87%E7%9A%84%E5%9D%91/20160815-AccessToken%E7%9A%84%E5%88%B7%E6%96%B0%E8%A7%84%E5%88%99%E5%9C%A8%E5%85%AC%E4%BC%97%E5%8F%B7%E3%80%81%E4%BC%81%E4%B8%9A%E5%8F%B7%E4%B8%AD%E6%98%AF%E4%B8%8D%E4%B8%80%E6%A0%B7%E7%9A%84.md)）

2. 此账号是否在多个站点使用，导致相互之间不断刷新争夺最新的AccessToken。

    这里说的多个站点也包括分布式、负载均衡的部署环境，这种情况下请使用统一的缓存对AccessToken进行管理。

3. 检查一下是否曾经将账号明文挂靠到第三方的平台（如有赞），第三方的平台也可能随时自动刷新AccessToken，
如果希望切断，可以更换Secret。

4. 如果上面两种情况都不存在，请检查账号是否泄漏，建议尽快到微信管理后台刷新Secret。

    微信的管理后台可以看到接口调用的次数，可以以此来判断是否还有其他人再刷新AccessToken。
