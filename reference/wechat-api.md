# Wechat Login For Web Apps

This feature lets users log into third-party sites on their computer by scanning a QR code displayed on the page, using their mobile phone. This reduces any friction new users may encounter in using your site, saving the time and hassle of having to remember yet another password.

WeChat Login for Web Apps is based on the OAuth 2.0 protocol. To use it, you must register a developer account via our management interface.

**How to register a developer account?**

* **China-market Official Account** [Register here!](https://mp.weixin.qq.com/cgi-bin/loginpage?t=wxm2-login&lang=zh_CN)  

**NOTE: Registering an account on this system will require a registered Mainland Chinese legal entity. The specific credentials and paperwork required depends on the type of entity the account will be associated with. Accounts can also be registered to individuals.**

* **International-market Official Account** [Register here!](http://apply.wechat.com/)

Once registered, you should get an **AppID** and **AppSecret** for your app by submitting your app on the portal. After your application has been approved, you can start using this API!

### Authorization Process

Basically, the overall process works like this:

1. The third party redirects the user to a page hosted by WeChat containing a QR code. After the user permits the third party site to access their account, WeChat will redirect the user back to the site, passing along a temporary **code** parameter in the query string.

2. Using this code parameter, along with the previously-obtained **AppID** and **AppSecret**, the third-party app can receive an **access\_token**.

3. Finally, the third party app can use this **access\_token** to call other APIs, such as the one for retrieving the user's profile info.

Hence, the steps to implement WeChat in your Web Apps are:

[Step 1: Getting the code parameter](/reference/wechat-api/step-1-getting-the-code-parameter.md) .

[Step 2: Passing the code to get the access token](/reference/wechat-api/step-2-passing-the-code-to-get-the-access_token.md).

[Step 3: Using your access token](/reference/wechat-api/step-3-using-your-access_token.md).





