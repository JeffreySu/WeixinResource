# AccessToken的刷新规则在公众号、企业号中是不一样的

> [时间：2016.08.15] [作者：@JeffreySu]

在公众号中，每次调用 https://api.weixin.qq.com/cgi-bin/token?xxx 接口，都会重新刷新AccessToken，
但在企业号中，同样调用获取AccessToken的接口，在短时间内获取到的AccessToken是相同的（被微信服务器缓存）。


## 另付企业号AccessToken限制

当你获取到AccessToken时，你的应用就可以成功调用企业号后台所提供的各种接口以管理或访问企业号后台的资源或给企业号成员发消息。

为了防止企业应用的程序错误而引发企业号服务器负载异常，默认情况下，每个企业号调用接口都有一定的频率限制，当超过此限制时，调用对应接口会收到相应错误码。

以下是当前默认的频率限制，企业号后台可能会根据运营情况调整此阈值：

基础频率：

每企业调用单个cgi/api不可超过1000次/分，30000次/小时

每ip调用单个cgi/api不可超过2000次/分，60000次/小时

每ip获取AccessToken不可超过300次/小时

发消息频率：

每企业不可超过200次/分钟；不可超过帐号上限数*30人次/天

创建帐号频率：

每企业创建帐号数不可超过帐号上限数*3/月

## 参考资料
公众号获取AccessToken：
https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421140183&token=&lang=zh_CN

企业号获取AccessToken：
http://qydev.weixin.qq.com/wiki/index.php?title=%E4%B8%BB%E5%8A%A8%E8%B0%83%E7%94%A8