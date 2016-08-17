# 通过OAuth获取OpenId

> [时间：2016.08.15] [作者：@JeffreySu] [关键字：OAuth,OpenId]

## 理解OAuth2.0

　　首先我们通过一张图片来了解一下OAuth2.0的运作模式：

  ![OAuth2.0](http://images.cnitblog.com/i/28384/201406/021337520528519.jpg)

　　从上图我们可以看到，整个过程进行了2次“握手”，最终利用授权的AccessToken进行一系列的请求，相关的过程说明如下：

A：由客户端向服务器发出验证请求，请求中一般会携带这些参数
* ID标识，例如appId
* 验证后跳转到的URL（redirectUrl）
* 状态参数（可选）
* 授权作用域scope（可选）
* 响应类型（可选）

B：服务器返回一个grant授权标识（微信默认情况下称之为code），类似于一个一次性的临时字符串密钥。如果在A中提供了redirectUrl，这里服务器会做一次跳转，带上grant和状态参数，访问redirectUrl。

C：客户端的redirectUrl对应页面，凭借grant再次发起请求，这次请求通常会携带一些敏感信息：
* ID标识
* 密码
* grant字符串（code）
* grant类型（可选，微信中默认为code）

D：服务器验证ID标识、密码、grant都正确之后，返回AccessToken（注意，这里的AccessToken和之前通用接口、高级接口介绍的AccessToken没有关系，不能交叉使用）

E：客户端凭借AccessToken请求一系列的API，在此过程中不再会携带appId，Secret，grant等敏感的信息。

F：服务器返回请求结果。

## 微信的OAuth2.0使用

　　了解了OAuth2.0的基本原理之后，我们来看一下OAuth2.0在微信中是如何运用的。

　　假设一个场景：用户进入了一个微信公众账号，随后通过消息中的链接，在微信内嵌浏览器中打开了一个游戏网页，这个游戏需要用户登录并且记录用户的游戏得分。

　　这种情况下我们有两种处理方式：

1. 让用户在网页中进行注册、登录（并且每次打开这个网页都可能要重新进行登录，因为微信内置浏览器的cookie保存时间非常短），这个当然是个很坑爹的设计。
2. 利用OAuth2.0。在用户进入这个页面的时候，先判断用户是否登录，如果没有，自动跳转到OAuth2.0授权页面，这个页面又自动进行了上述ABCD一系列验证，再通过EF得到用户的OpenId甚至更加详细的信息（包括头像），自动完成登录（或必要的注册）过程，随后用户以登录状态直接进入游戏。
　　可以看出，使用OAuth2.0大幅度提高了用户体验，并且可以自动绑定识别用户微信OpenId。

　　需要注意的是，上面提到的“OAuth2.0授权页面”还有两种形式：

1. 当请求A中的Scope为snsapi_base时，整个授权过程自动完成，用户的客户端不会有任何中间页面显示，但是授权的结果仅能获取用户的OpenId（不管用户是否已关注，当然如果用户是关注用户，再次利用高级接口中的用户信息接口，利用OpenId获取用户资料也是可以的，只不过绕了几个弯）
2. 当请求A中的Scope为snsapi_userinfo时，需要提供一个授权页面（类似很多网站利用微博账号、QQ号登陆的那种授权），仅当用户同意之后，立即获取到用户的详细信息，这里的用户可以是关注用户，也可以是未关注用户，返回的内容一致。
　　也就是说，snsapi_base的方法可以“神不知鬼不觉”地获取用户OpenId，全自动完成登录注册过程，但是信息量有限；snsapi_userinfo需要用户在指定界面上授权之后，自动完成整个过程，这个授权有一个时间段，超过时间后需要重新询问用户。

## 注意点
1. 必须是通过认证的服务号才可以使用OAuth接口。
2. 接口中用到的AccessToken和高级接口（包括通用接口）中用到的AccessToken互不相关，即使他们都是通过相同的AppId和Secret得到的。
3. 目前官方的授权页面不是100%稳定，有时候需要多点几次才能顺利通过，如果发现此类情况，需要做一些判断反复请求，至少在表面上可以不让用户看到错误页面。
4. 出于安全，在使用OAuth2.0之前，需要进入到微信后台的【我的服务】对回调页面的域名进行设置：
![](http://images.cnitblog.com/i/28384/201406/021417583646945.png)

![](http://images.cnitblog.com/i/28384/201406/021421110523044.png)

## Senparc.Weixin.MP OAuth2.0接口

　　源文件文件夹：Senparc.Weixin.MP/AdvancedAPIs/OAuth

　　源代码中相关方法如下：

``` C#
namespace Senparc.Weixin.MP.AdvancedAPIs
{
    //官方文档：http://mp.weixin.qq.com/wiki/index.php?title=%E7%BD%91%E9%A1%B5%E6%8E%88%E6%9D%83%E8%8E%B7%E5%8F%96%E7%94%A8%E6%88%B7%E5%9F%BA%E6%9C%AC%E4%BF%A1%E6%81%AF#.E7.AC.AC.E4.B8.80.E6.AD.A5.EF.BC.9A.E7.94.A8.E6.88.B7.E5.90.8C.E6.84.8F.E6.8E.88.E6.9D.83.EF.BC.8C.E8.8E.B7.E5.8F.96code

    /// <summary>
    /// 应用授权作用域
    /// </summary>
    public enum OAuthScope
    {
        /// <summary>
        /// 不弹出授权页面，直接跳转，只能获取用户openid
        /// </summary>
        snsapi_base,
        /// <summary>
        /// 弹出授权页面，可通过openid拿到昵称、性别、所在地。并且，即使在未关注的情况下，只要用户授权，也能获取其信息
        /// </summary>
        snsapi_userinfo
    }

    public static class OAuth
    {
        /// <summary>
        /// 获取验证地址
        /// </summary>
        /// <param name="appId"></param>
        /// <param name="redirectUrl"></param>
        /// <param name="state"></param>
        /// <param name="scope"></param>
        /// <param name="responseType"></param>
        /// <returns></returns>
        public static string GetAuthorizeUrl(string appId, string redirectUrl, string state, OAuthScope scope, string responseType = "code")
        {
            var url =
                string.Format("https://open.weixin.qq.com/connect/oauth2/authorize?appid={0}&redirect_uri={1}&response_type={2}&scope={3}&state={4}#wechat_redirect",
                                appId, redirectUrl.UrlEncode(), responseType, scope, state);

            /* 这一步发送之后，客户会得到授权页面，无论同意或拒绝，都会返回redirectUrl页面。
             * 如果用户同意授权，页面将跳转至 redirect_uri/?code=CODE&state=STATE。这里的code用于换取access_token（和通用接口的access_token不通用）
             * 若用户禁止授权，则重定向后不会带上code参数，仅会带上state参数redirect_uri?state=STATE
             */
            return url;
        }

        /// <summary>
        /// 获取AccessToken
        /// </summary>
        /// <param name="appId"></param>
        /// <param name="secret"></param>
        /// <param name="code">code作为换取access_token的票据，每次用户授权带上的code将不一样，code只能使用一次，5分钟未被使用自动过期。</param>
        /// <param name="grantType"></param>
        /// <returns></returns>
        public static OAuthAccessTokenResult GetAccessToken(string appId, string secret, string code, string grantType = "authorization_code")
        {
            var url =
                string.Format("https://api.weixin.qq.com/sns/oauth2/access_token?appid={0}&secret={1}&code={2}&grant_type={3}",
                                appId, secret, code, grantType);

            return CommonJsonSend.Send<OAuthAccessTokenResult>(null, url, null, CommonJsonSendType.GET);
        }

        /// <summary>
        /// 刷新access_token（如果需要）
        /// </summary>
        /// <param name="appId"></param>
        /// <param name="refreshToken">填写通过access_token获取到的refresh_token参数</param>
        /// <param name="grantType"></param>
        /// <returns></returns>
        public static OAuthAccessTokenResult RefreshToken(string appId, string refreshToken, string grantType = "refresh_token")
        {
            var url =
                string.Format("https://api.weixin.qq.com/sns/oauth2/refresh_token?appid={0}&grant_type={1}&refresh_token={2}",
                                appId, grantType, refreshToken);

            return CommonJsonSend.Send<OAuthAccessTokenResult>(null, url, null, CommonJsonSendType.GET);
        }

        public static OAuthUserInfo GetUserInfo(string accessToken,string openId)
        {
            var url = string.Format("https://api.weixin.qq.com/sns/userinfo?access_token={0}&openid={1}",accessToken,openId);
            return CommonJsonSend.Send<OAuthUserInfo>(null, url, null, CommonJsonSendType.GET);
        }
    }
}
```

　　具体的示例方法见
[Senparc.Weixin.MP.Sample/Controllers/OAuth2Controller.cs](https://github.com/JeffreySu/WeiXinMPSDK/blob/master/src/Senparc.Weixin.MP.Sample/Senparc.Weixin.MP.Sample/Controllers/OAuth2Controller.cs)，
以及对应[视图](https://github.com/JeffreySu/WeiXinMPSDK/tree/master/src/Senparc.Weixin.MP.Sample/Senparc.Weixin.MP.Sample/Views/OAuth2)的代码。

## 参考资料
《Senparc.Weixin.MP SDK 微信公众平台开发教程（十二）：OAuth2.0说明》
http://www.cnblogs.com/szw/archive/2013/05/14/weixin-course-index.html