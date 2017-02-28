# Step 2: Passing the code to get the access\_token

After the user is redirected back to your site, you can take the passed ‘code’ and call the access\_token API as follows:

[https://api.weixin.qq.com/sns/oauth2/access\_token?appid=APPID&secret=SECRET&code=CODE&grant\_type=authorization\_code](https://api.weixin.qq.com/sns/oauth2/access_token?appid=APPID&secret=SECRET&code=CODE&grant_type=authorization_code)

**Important Note:**

* If you have an International Official Account, use `api.wechat.com`.
* If you have a China Official Account, use `api.weixin.qq.com`.

**Parameters**

  
![](/assets/wechatapirequestparameters.PNG)

### Response field

Example:

```json
{   
    "access_token":"ACCESS_TOKEN",
    "expires_in":7200,
    "refresh_token":"REFRESH_TOKEN",
    "openid":"OPENID",
    "scope":"SCOPE",
    "unionid": "o6_bmasdasdsad6_2sgVt7hMZOPfL" 
}
```

**Parameters**

![](/assets/responsefieldparameters.PNG)

An error message will look something like this:

```json
{
    "errcode":40029,
    "errmsg":"invalid code"
}
```

For more info regarding the error messages, refer [here](http://open.wechat.com/cgi-bin/newreadtemplate?t=overseas_open/docs/oa/basic-info/return-codes#basic-info_return-codes).

[Go to step 3](/reference/wechat-api/step-3-using-your-access_token.md)

