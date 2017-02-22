

#Implementation of Facebook Login

To implement this, we need to first implement the Facebook SDK for JavaScript. You don't have to download anything, just simply include a piece of regular JavaScript in the HTML that will load the SDK into your pages.

The following snippet of code will give the basic version of the SDK where the options are set to their most common defaults.

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

And now lets add the Facebook login interface. You will need to paste the code below in the body section of your html code.

```html
    <fb:login-button scope="public_profile,email" onlogin="checkLoginState();">
</fb:login-button>
<div id="status">
</div>
```

Next, we would like to add the function that handles the response and alters the page contents based on the type of response. Examples of code snippets are given below:

```js
// This is called with the results from from FB.getLoginStatus().
function statusChangeCallback(response) {
// The response object is returned with a status field that lets the app know the current login status of the person.
if (response.status === 'connected') {
    console.log('Welcome! Fetching your information.... ');
    FB.api('/me', function(response) {
        console.log('Successful login for: ' + response.name);
        document.getElementById('status').innerHTML = 'Thanks for logging in, ' + response.name + '!';
});
} else if (response.status === 'not_authorized') {
// The person is logged into Facebook, but not your app.
    document.getElementById('status').innerHTML = 'Please log ' + 'into this app.';
} else {
// The person is not logged into Facebook, so we're not sure if they are logged into this app or not.
    document.getElementById('status').innerHTML = 'Please log ' + 'into Facebook.';
    }
}
```
As you can see, the above function receives a response variable and checks it’s status. If it is connected it fetches the logged in users info and outputs this information in the console of your browser, that area is where you could build more onto this script to handle the data. You’ll also notice the `not_authorized` status. When the login is not authorized, this function changed the html on your page to ask you to log in. But how does this function get used? In the next code example, this is handled when someone clicked on the Facebook button on the page. Notice the `onlogin=”checkLoginState();` in your body html code for the Facebook button.

```js
// This function is called when someone finishes with the Login Button.
function checkLoginState() {
    FB.getLoginStatus(function(response) {
    statusChangeCallback(response);
    });
}
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


**Here, have a look on the full implementation [sample codes](/reference/facebook/full-code-sample.md).**



