
#Authenticate with a backend server

After successfully sign in, retrieve the user's ID token:

```js
function onSignIn(googleUser) {
  var id_token = googleUser.getAuthResponse().id_token;
  ...
}
```

Then, send the ID token to your server with an HTTPS POST request:
```js
var xhr = new XMLHttpRequest();
xhr.open('POST', 'https://yourbackend.example.com/tokensignin');
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
xhr.onload = function() {
  console.log('Signed in as: ' + xhr.responseText);
};
xhr.send('idtoken=' + id_token);
```

###Next, validate the integrity of the ID token
After you receive the ID token by HTTPS POST, you must verify the integrity of the token. There is already a library from Google called **Google API Client Library** to help developers to validate the ID tokens in a production environment.

There are many different languages that the library is written which you can access them [here](https://developers.google.com/api-client-library/).

Example of implementing the library in Node.js:

**Install the library**
```node
npm install google-auth-library --save
```

**Then, call the `verifyIdToken()`function. For example**
```js
var GoogleAuth = require('google-auth-library');
var auth = new GoogleAuth;
var client = new auth.OAuth2(CLIENT_ID, '', '');
client.verifyIdToken(
    token,
    CLIENT_ID,
    // Or, if multiple clients access the backend:
    //[CLIENT_ID_1, CLIENT_ID_2, CLIENT_ID_3],
    function(e, login) {
      var payload = login.getPayload();
      var userid = payload['sub'];
      // If request specified a G Suite domain:
      //var domain = payload['hd'];
    });
```
The `verifyIdToken` function verifies the JWT signature, the `aud` claim, the `exp` claim, and the `iss` claim.


**Calling the tokeninfo endpoint**

An easy way to validate an ID token for debugging and low-volume use is to use the `tokeninfo` endpoint. Calling this endpoint involves an additional network request that does most of the validation for you, but introduces some latency and the potential for network errors.

To validate an ID token using the `tokeninfo` endpoint, make an HTTPS POST or GET request to the endpoint, and pass your ID token in the `id_token` parameter. For example, to validate the token "XYZ123", make the following GET request:

```json
https://www.googleapis.com/oauth2/v3/tokeninfo?id_token=XYZ123
```

If the token is properly signed and the `iss` and `exp` claims have the expected values, you will get a HTTP 200 response, where the body contains the JSON-formatted ID token claims. Here's an example response:

```json
{
 // These six fields are included in all Google ID Tokens.
 "iss": "https://accounts.google.com",
 "sub": "110169484474386276334",
 "azp": "1008719970978-hb24n2dstb40o45d4feuo2ukqmcc6381.apps.googleusercontent.com",
 "aud": "1008719970978-hb24n2dstb40o45d4feuo2ukqmcc6381.apps.googleusercontent.com",
 "iat": "1433978353",
 "exp": "1433981953",

 // These seven fields are only included when the user has granted the "profile" and
 // "email" OAuth scopes to the application.
 "email": "testuser@gmail.com",
 "email_verified": "true",
 "name" : "Test User",
 "picture": "https://lh4.googleusercontent.com/-kYgzyAWpZzJ/ABCDEFGHI/AAAJKLMNOP/tIXL9Ir44LE/s99-c/photo.jpg",
 "given_name": "Test",
 "family_name": "User",
 "locale": "en"
}
```

