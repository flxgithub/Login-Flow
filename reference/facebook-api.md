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

* ##### Use the login button

It's easy to include the Login Button into the page. You can just get the basic button code [here](https://developers.facebook.com/docs/facebook-login/web#loginbutton). For information on customizing the Login Button, see [this](https://developers.facebook.com/docs/facebook-login/web/login-button).

or you can log people in using

* ##### The Login Dialog from the JavaScript SDK

For apps that wanted to use their own button, you can invoke the Login Dialog with a simple call to `FB.login()`

As noted in the references docs down below, it results in a popup window showing the Login dialog, and therefore should only be invoked as a result of someone clicking an HTML button \(so that the popup isn't blocked by browsers\).

There's an optional `scope`parameter that can be passed along with the function call that is a comma separated list of [permissions](https://developers.facebook.com/docs/facebook-login/permissions) to request from the person using the app. Here's an example of the code

```js
FB.login(function(response){
    //handle the response
}, {scope: 'public_profile,email'});
```

##### Handle Login Dialog Response

At this point in the login flow, your app displays the login dialog, which gives you the choice of whether to cancel or to enable the app to access their data.

Whatever choice people make, the browser returns to the app and response data indicating whether they're connected or cancelled is sent to your app. When you uses the JavaScript SDK, it returns an `authResponse` object to the callback specified when you made the `FB.login()` call:

This response can be detected and handled within the `FB.login` call, like the example below:

```js
FB.login(function(response) {
    if (response.status === 'connected'){

    } else if (response.status === 'not_authorized') {

    } else {

    }
});
```

### 3. Log People Out

You can log people out of your app by attaching the JavaScript SDK function `FB.logout` to a button or a link, as follows:

```js
FB.logout(function(response){
    //user is now logged out
});
```

**Note: This function call may also log the person out of Facebook. **Consider the 3 scenarios below:

1. A person logs into Facebook, then logs into your app. Upon logging out from your app, the person is still logged into Facebook.
2. A person logs into your app and into Facebook as part of your app's login flow. Upon logging out from your app, the user is also logged out of Facebook.
3. A person logs into another app and into Facebook as part of the other app's login flow, then logs into your app. Upon logging out from either app, the user is logged out of Facebook.

**Additionally, logging out is not the same as revoking login permission \(removing previously granted authentication\), which can be performed separately. Because of this your app should be built in such a way that it doesn't automatically force people who have logged out back to the Login dialog.**



### Full Code Example

This code will load and initialize the JavaScript SDK in your HTML page. Use your app ID where indicated.

```js
<!DOCTYPE html>
<html>
<head>
<title>Facebook Login JavaScript Example</title>
<meta charset="UTF-8">
</head>
<body>
<script>
  // This is called with the results from from FB.getLoginStatus().
  function statusChangeCallback(response) {
    console.log('statusChangeCallback');
    console.log(response);
    // The response object is returned with a status field that lets the
    // app know the current login status of the person.
    // Full docs on the response object can be found in the documentation
    // for FB.getLoginStatus().
    if (response.status === 'connected') {
      // Logged into your app and Facebook.
      testAPI();
    } else if (response.status === 'not_authorized') {
      // The person is logged into Facebook, but not your app.
      document.getElementById('status').innerHTML = 'Please log ' +
        'into this app.';
    } else {
      // The person is not logged into Facebook, so we're not sure if
      // they are logged into this app or not.
      document.getElementById('status').innerHTML = 'Please log ' +
        'into Facebook.';
    }
  }

  // This function is called when someone finishes with the Login
  // Button.  See the onlogin handler attached to it in the sample
  // code below.
  function checkLoginState() {
    FB.getLoginStatus(function(response) {
      statusChangeCallback(response);
    });
  }

  window.fbAsyncInit = function() {
  FB.init({
    appId      : '{your-app-id}',
    cookie     : true,  // enable cookies to allow the server to access 
                        // the session
    xfbml      : true,  // parse social plugins on this page
    version    : 'v2.8' // use graph api version 2.8
  });

  // Now that we've initialized the JavaScript SDK, we call 
  // FB.getLoginStatus().  This function gets the state of the
  // person visiting this page and can return one of three states to
  // the callback you provide.  They can be:
  //
  // 1. Logged into your app ('connected')
  // 2. Logged into Facebook, but not your app ('not_authorized')
  // 3. Not logged into Facebook and can't tell if they are logged into
  //    your app or not.
  //
  // These three cases are handled in the callback function.

  FB.getLoginStatus(function(response) {
    statusChangeCallback(response);
  });

  };

  // Load the SDK asynchronously
  (function(d, s, id) {
    var js, fjs = d.getElementsByTagName(s)[0];
    if (d.getElementById(id)) return;
    js = d.createElement(s); js.id = id;
    js.src = "//connect.facebook.net/en_US/sdk.js";
    fjs.parentNode.insertBefore(js, fjs);
  }(document, 'script', 'facebook-jssdk'));

  // Here we run a very simple test of the Graph API after login is
  // successful.  See statusChangeCallback() for when this call is made.
  function testAPI() {
    console.log('Welcome!  Fetching your information.... ');
    FB.api('/me', function(response) {
      console.log('Successful login for: ' + response.name);
      document.getElementById('status').innerHTML =
        'Thanks for logging in, ' + response.name + '!';
    });
  }
</script>

<!--
  Below we include the Login Button social plugin. This button uses
  the JavaScript SDK to present a graphical Login button that triggers
  the FB.login() function when clicked.
-->

<fb:login-button scope="public_profile,email" onlogin="checkLoginState();">
</fb:login-button>

<div id="status">
</div>

</body>
</html>
```







