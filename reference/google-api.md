# Google API Documentation

Before integrating Google Sign-In into the website, it's a must that we  have a Google API Console project. In the project, you create a client ID, which you need to call the sign-in API.

**To create a Google API Console project and client ID, follow these steps:**

1. Go to the [Google API Console](https://console.developers.google.com/projectselector/apis/library).
2. From the project drop-down, select an existing project, or create a new one by selecting the **Create a new project.**

   1. **Note: Use a single project to hold all platform instances of your app \(Android, iOS, web, etc.\), each with a different Client ID**.

   2. To know more about how to manage projects in the console, click [here](https://support.google.com/cloud/answer/6158853).

3. In the sidebar under "API Manager", select **Credentials**, then select the **OAuth consent screen **tab.

   1. Choose an **Email Address**, specify a **Product Name**, and press **Save**.

4. In the **Credentials** tab, select the **New credentials  **drop-down list, and choose **OAuth client ID.**

5. Under **Application type**, select **Web application**. Register the origins from which your app is allowed to access the Google APIs, as follows. An origin is a unique combination of protocol, hostname, and port.

   1. In the **Authorized JavaScript origins **field, enter the origin for the app. Multiple origins can be entered for the app to run on different protocols, domains, or subdomains. **Wildcards is extremely prohibited. **In the example below, the second URL could be a production URL.

   2. ```js
      http://localhost:8080
      https://myproductionurl.example.com
      ```

      ii. The **Authorized redirect URI **field does not require a value. Redirect URIs are not used with JavaScript APIs.

   3. Press the **Create **button.

6. From the resulting **OAuth client dialog box**, copy the **Client ID. **The Client ID lets the app access enabled Google APIs.

Now we can proceed to adding Google Sign-In to the website. [Add Google Sign-In](https://developers.google.com/identity/sign-in/web/sign-in)





