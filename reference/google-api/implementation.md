# Integrating Google Sign-In

Google Sign-In manages the OAuth 2.0 flow and token lifecycle, simplifying your integration with Google APIs. A user always has the option to revoke access to an application at any time.

This document describes how to complete a basic Google Sign-In integration.

There are a few steps need to be followed here. First of all you should..

#### Load the Google Platform Library

You must include the Google Platform Library on your web pages that integrate Google Sign-In

```js
<script src="https://apis.google.com/js/platform.js" async defer></script>
```

#### Specify your app's client ID

Specify the client ID you created for your app in the Google Developers Console with the `google-signin-client_id` meta element.

```js
<meta name="google-signin-client_id" content="YOUR_CLIENT_ID.apps.googleusercontent.com">
```
####Add a Google Sign-In button
The easiest way to add a Google Sign-In button to your site is to use an automatically rendered sign-in button. With only a few lines of code, you can add a button that automatically configures itself to have the appropriate text, logo, and colors for the sign-in state of the user and the scopes you request.

To create a Google Sign-In button that uses the default settings, add a `div` element with the class `g-signin2` to your sign-in page:

```html
<div class="g-signin2" data-onsuccess="onSignIn"></div>
```

The following is an example of the default Google Sign-In button:

![](/assets/googlesigninbutton.PNG)

####Get Profile Information
After you have signed in a user with Google using the default scopes, you can access the user's Google ID, name, profile URL, and email address.

To retrieve profile information for a user, use the `getBasicProfile()` method.

```js
function onSignIn(googleUser) {
  var profile = googleUser.getBasicProfile();
  console.log('ID: ' + profile.getId()); // Do not send to your backend! Use an ID token instead.
  console.log('Name: ' + profile.getName());
  console.log('Image URL: ' + profile.getImageUrl());
  console.log('Email: ' + profile.getEmail()); // This is null if the 'email' scope is not present.
}
```

####Sign Out
You can enable users to sign out of your app without signing out of Google by adding a sign-out button or link to your site. To create a sign-out link, attach a function that calls the `GoogleAuth.signOut()` method to the link's onclick event.

```html
<a href="#" onclick="signOut();">Sign out</a>
<script>
  function signOut() {
    var auth2 = gapi.auth2.getAuthInstance();
    auth2.signOut().then(function () {
      console.log('User signed out.');
    });
  }
</script>
```

If you use Google Sign-In with an app or site that communicates with a backend server, you might need to identify the currently signed-in user on the server. To do so securely, after a user successfully signs in, send the user's ID token to your server using HTTPS. Then, on the server, verify the integrity of the ID token and retrieve the user's ID from the `sub` claim of the ID token. You can use user IDs transmitted in this way to safely identity the currently signed-in user on the backend. [Look here](/reference/google-api/authenticate-with-backend-server.md).


