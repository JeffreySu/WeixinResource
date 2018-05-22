# 错误信息：通过公众号接口创建会员卡：Invalid Card Type ，直接在公众号后台创建提示：系统繁忙，请稍后再试

> [时间：2018.05.21] [关键字：invalid card type]

## 现象

通过api接口创建会员卡提示：Invalid Card Type ，通过微信公众号后台创建会员卡提示：系统繁忙，请稍后再试。

## 原因

所选类目不再微信会员开开放类目范围内。

从微信开发这文档中会员卡接口里面的【会员卡公告】链接点开后的会员开开放类目为下图：

![xxx](http://mmbiz.qpic.cn/mmbiz_png/PiajxSqBRaEIQxibpLbyuSKwrjmXRSUicP72QRRJt9RicjX94XzKV0mlUkbbyE7HKqmAPsTFfcib5som8sm0ia2uxW0w/0?wx_fmt=png)

上图地址：https://mp.weixin.qq.com/cgi-bin/announce?action=getannouncement&key=1461052555&version=1&lang=zh_CN&platform=2

官方客服给出的另一份会员卡开放类目范围为如下表格：

![xxx](https://github.com/suifengshiqu/MdImages/blob/master/Images/cardtype.png?raw=true)

上图表格所在的链接地址如下：

https://mp.weixin.qq.com/cgi-bin/readtemplate?t=cardticket/faq_tmpl&type=info&amp;token=5304410&lang=zh_CN#3dot2

注意：上面链接地址打开默认显示为第三方代制的子商户类目范围，下拉页面即可找到上面的表格。

微信会员卡在官方两份文档里面的开放类目不一致，导致申请卡券功能后无法创建会员卡，请大家在申请时一定要注意文档中的类目，尽量使用两份文档中同时存在的类目，
由于类目申请错误，几经周折终于邮箱联系上微信卡券客服（人工电话从来没有打通过），但对方只是表示无法当前类目无法修改，也无具体的解决办法，希望大家在做会员卡开发的时候多注意一下这里的问题。
![email](https://github.com/suifengshiqu/MdImages/blob/master/Images/email.png?raw=true)
