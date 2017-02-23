
#Step 1: Getting the code parameter

When a user selects the “Login with WeChat” option, the third-party site should link the user to a URL following the format below. This page will display a QR code that the user can scan.

https://open.weixin.qq.com/connect/qrconnect?appid=APPID&redirect_uri=REDIRECT_URI&response_type=code&scope=SCOPE&state=STATE#wechat_redirect

If an error appears after loading this URL, double-check the parameters passed. The domain in the **redirect_ui** must match what was entered and approved for your app on our developer portal. Also, be sure to use the scope **"snsapi_login"**.

###Parameters
    
![](/assets/wechatparameters.PNG)

###Response
If the user authorizes the login, the parameters passed will include both code and state information:

`redirect_uri?code=CODE&state=STATE`

If the user refuses to authorize the login, the parameters passed in the query string will only include state:

`redirect_uri?state=STATE`

###Example WeChat Login Flow

The commerce website YiHaoDian uses this API to allow users to login using a WeChat account. First, the user opens this page on their site:

https://passport.yhd.com/wechat/login.do

Afterwards, the site redirects the user to something like:

https://open.weixin.qq.com/connect/qrconnect?appid=wxbdc5610cc59c1631&redirect_uri=https%3A%2F%2Fpassport.yhd.com%2Fwechat%2Fcallback.do&response_type=code&scope=snsapi_login&state=3d6be0a4035d839573b04816624a415e#wechat_redirect

Then, once the user scans the code on their phone and authorizes the login, we redirect them back to YiHaoDian, passing along the appropriate parameters:

https://passport.yhd.com/wechat/callback.do?code=CODE&state=3d6be0a4035d839573b04816624a415e

###Customizing Your Login Page
To make the WeChat login process better match your website’s look and feel, we also support another way to display the QR code on your page, using JavaScript.

First, include this line in your site’s `head` (this also supports HTTPS):

```html
<script src="http://res.wx.qq.com/connect/zh_CN/htmledition/js/wxLogin.js"></script>
```

Next, in your code, instantiate WxLogin, passing the following parameters:
```js
var obj = new WxLogin({
          id:"login_container",
          appid: "",
          scope: "",
          redirect_uri: "",
          state: "",
          style: "",
          href: ""
      });
```

**Parameters**

![](/assets/logincustomizeparameters.PNG)

[Go to step 2](/reference/wechat-api/step-2-passing-the-code-to-get-the-access_token.md)


