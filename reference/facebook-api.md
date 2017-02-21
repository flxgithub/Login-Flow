# Facebook Login API Documentation

## Facebook Login API Documentation {#facebook-login-api-documentation}

In our case we will use Facebook Login for Web. It enables people to sign into our web page with their Facebook credentials. To implement this, we need to first implement the Facebook SDK for Javascript. You don't have to download anything, just simply include a piece of regular JavaScript in the HTML that will load the SDK into your pages.

The following snippet of code will give the basic version of the SDK where the options are set to their most common defaults.

It should be inserted directly after the opening`<body>`tag on each page you want to load it.

```js
    <script>
        window.fbAsyncInit = function() {
            FB.init({
                appId : 'your-app-id',
                xfbml : true,
                version : 'v2.8'
            });
            FB.AppEvents.logPageView();
        };

        (function(d, s, id){
            var js, fjs = d.getElementsByTagName(s)[0];
            if (d.getElementById(id)) {return;}
            js = d.createElement(s); js.id = id;
            js.src = "//connect.facebook.net/en_US/sdk.js";
            fjs.parentNode.insertBefore(js, fjs);
        }(document, 'script', 'facebook-jssdk'));
    </script>
```

This code will load and initialize the SDK. The`your-app-id`must be replaced with the ID of your own Facebook App. You can find this ID [here](https://developers.facebook.com/apps).

There are some steps that needed to implement Facebook Login

### 1. Check Login Status

The first step when loading website is to check whether is the user logged into your website with Facebook Login. The process of  the checking is started with a call to `FB.getLoginStatus`. This function will trigger a call to Facebook to get the login status and call your callback function with the results.

Below is the example of the code that's run during page load to check a person's login status:

```js
FB.getLoginStatus(function(response){
    statusChangeCallback(response);
});
```

The `response`object that's provided to your callback contains a number of fields:

```js
{
    status: 'connected',
    authResponse: {
        accessToken: '...',
        expiresIn: '...',
        signedRequest: '...',
        userID: '...'
    }
}
```

`status` specifies the login status of the person using the app. It can be one of the following:

* `connected` - The person is logged into Facebook, and has logged into your app.
* `not_authorized` - The person is logged into Facebook, but not your app.
* `unknown` - The person is not logged into Facebook, so you don't know if they've logged into your app. \(Or `FB.logout()` was called before and therefore, it cannot connect to Facebook\).
* `authResponse` - is included if the status is `connected` and is made up of the following:
  * `accessToken` - Contains an access token for the person using the app.
  * `expiresIn` - Indicates the UNIX time when the token expires and needs to be renewed.
  * `signedRequest` - A signed parameter that contains information about the person using the app.
  * `userID` - is the ID of the person using the app

Once the app knows the login status of the person using it, it can do one of the following:

* If the user is logged into Facebook and your app, redirect them to your app's logged in experience.
* If the person isn't logged into your app or Facebook, prompt them with the Login dialog with `FB.Login()` or show them the login button.

### 2. Log People In

If the users aren't logged into Facebook, they'll first be prompted to login and then move on to logging into the app. The JavaScript SDK already handles this automatically, so nothing extra had to be done to enable this behaviour.

There are two ways to log someone in:

* #### Use the Login Button

It's easy to include the Login Button into the page. You can just get the basic button code [here](https://developers.facebook.com/docs/facebook-login/web#loginbutton). For information on customizing the Login Button, see [this](https://developers.facebook.com/docs/facebook-login/web/login-button).










