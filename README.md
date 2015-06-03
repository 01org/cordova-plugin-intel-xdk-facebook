intel.xdk.facebook
==================

Integrate your application with Facebook.

>   _This Intel XDK Cordova plugin and API has been deprecated. A suggested
>   alternative Cordova plugin for use on Android and iOS platforms is available
>   as a
>   [third-party plugin](http://wizcorp.tumblr.com/post/91812168412/the-semi-official-facebook-
phonegap-plugin-comes-to).
>   Coverage for all platforms is available using
>   [this JavaScript micro
library](http://coenraets.org/blog/2014/04/facebook-phonegap-cordova-without-
plugin/)
>   (this is not a plugin). In addition, a general-purpose OAuth2 JavaScript
>   library (no plugin required)
>   [named "HelloJS"](http://adodson.com/hello.js) can be used to log into
>   Facebook (as well as a variety of other services)._

Description
-----------

This object gives applications the capability to log users into Facebook in
order to access their Facebook Graph as well as to post to the news feed and
share request dialogs.

For more information on on how to use the Facebook JavaScript API commands in a
native application, stop by this
[Facebook Integration](http://software.intel.com/en-us/html5/articles/integrating-facebook-functionality-into-hybrid-apps-using-the-intel-xdk)
article.

### Methods

-   [enableFrictionlessRequests](#enablefrictionlessrequests) — This command
    ensures that news feed requests are "frictionless" in that it enables users
    to send requests to specific friends without having to click on a pop-up
    confirmation dialog
-   [login](#login) — This method logs the user into Facebook
-   [logout](#logout) — This method logs the user out of Facebook
-   [requestWithGraphAPI](#requestwithgraphapi) — This method makes calls to the
    new Graph Facebook API
-   [requestWithRestAPI](#requestwithrestapi) — This method makes calls to the
    older REST Facebook API
-   [showAppRequestDialog](#showapprequestdialog) — This method displays the
    Facebook application request dialog
-   [showNewsFeedDialog](#shownewsfeeddialog) — This method displays the
    Facebook news feed submission dialog

### Events

-   [intel.xdk.facebook.busy](#busy) — Fired if a second Facebook request is
    made while a previous request is still pending
-   [intel.xdk.facebook.dialog.complete](#dialog.complete) — This event is fired
    in response to a news feed or app request dialog
-   [intel.xdk.facebook.dialog.fail](#dialog.fail) — This event is fired if
    Facebook tries to log the user on during a dialog request, but the login
    fails
-   [intel.xdk.facebook.login](#login) — Fired after logging a user into
    Facebook
-   [intel.xdk.facebook.logout](#logout) — Fired after logging a user out of
    Facebook
-   [intel.xdk.facebook.request.response](#request.response) — Fired in response
    to a Facebook API request
-   [intel.xdk.facebook.session.invalidate](#session.invalidate) — Fired if the
    Facebook session is invalidated

Methods
-------

### enableFrictionlessRequests

This command ensures that news feed requests are "frictionless" in that it
enables users to send requests to specific friends without having to click on a
pop-up confirmation dialog

```javascript
intel.xdk.facebook.enableFrictionlessRequests();
```

#### Description

This command ensures that news feed requests are "frictionless" in that it
enables users to send requests to specific friends without having to click on a
pop-up confirmation dialog. This command option is for iOS only since Android
requests are automatically set to be "frictionless".

#### Platforms

-   Apple iOS
-   Google Android

### login

This method logs the user into Facebook

```javascript
intel.xdk.facebook.login(permissions);
```

#### Description

This command is used to log the user into Facebook. When it is run, a dialog
generated by Facebook should appear allowing the user to enter their
credentials. Depending on the result of that interaction with Facebook, the
application will receive a JavaScript event (intel.xdk.facebook.login)
indicating whether the login was successful or not. This method will log the
user in with a series of default permissions if no permissions parameter is
specified.

#### Platforms

-   Apple iOS
-   Google Android

#### Events

-   **[intel.xdk.facebook.login](#login):** This event is fired upon completion
    of the interaction with Facebook and will indicate either successful login
    or failure.

#### Example

```javascript
alert("about to log into facebook");
document.addEventListener("intel.xdk.facebook.login",function(e){
        if (e.success == true)
        { console.log("Facebook Log in Successful"); }
        else
        { console.log("Unsuccessful Login"); }
},false);
intel.xdk.facebook.login("publish_stream,publish_actions,offline_access");
```

### logout

This method logs the user out of Facebook

```javascript
intel.xdk.facebook.logout();
```

#### Platforms

-   Apple iOS
-   Google Android

#### Events

-   **[intel.xdk.facebook.logout](#logout):** This event is fired upon
    completion of Facebook logout

#### Example

```javascript
alert("logging out of facebook");
document.addEventListener("intel.xdk.facebook.logout",function(e){
        if (e.success == true)
        { console.log("Logged out of Facebook"); }
        else
        { console.log("Unsuccessful Logout"); }
},false);
intel.xdk.facebook.logout();
```

### requestWithGraphAPI

This method makes calls to the new Graph Facebook API

```javascript
intel.xdk.facebook.requestWithGraphAPI(path, method, parameters);
```

#### Description

This method is used to make a request from the newer Facebook Graph API. For
more information on this particular API, see Facebook’s documentation
[here](http://developers.facebook.com/docs/reference/api/).

#### Platforms

-   Apple iOS
-   Google Android

#### Parameters

-   **path:** The Facebook Graph PATH to execute
-   **method:** The request method to call the Facebook Graph API, typically GET
    or POST
-   **parameters:** Any parameters required for the request

#### Events

-   **[intel.xdk.facebook.request.response](#requestresponse):** This event is
    fired upon completion of a Facebook API request.

#### Example

```javascript
var facebookUserID = "me";  //me = the user currently logged into Facebook
document.addEventListener("intel.xdk.facebook.request.response",function(e) {
    console.log("Facebook User Friends Data Returned");
    if (e.success == true) {
        var data = e.data.data;
        var outHTML = "";

        for (var r=0; r< data.length; r++) {
        outHTML += "<img src='http://graph.facebook.com/" + data[r]["id"] +
            "/picture' info='" + data[r]["name"] + "' />";

        }
        document.getElementsByTagName("body")[0].innerHTML = outHTML;
        document.removeEventListener("intel.xdk.facebook.request.response");
    }
},false);
intel.xdk.facebook.requestWithGraphAPI(facebookUserID + "/friends","GET","");
```

### requestWithRestAPI

This method makes calls to the older REST Facebook API

```javascript
intel.xdk.facebook.requestWithRestAPI(command, method, parameters);
```

#### Description

This method is used to make a request using the older Facebook REST API. This
API is being deprecated by Facebook, and may not be available in the future. For
more information on Facebook’s REST API see the Facebook documentation
[here](http://developers.facebook.com/docs/reference/rest/).

#### Platforms

-   Apple iOS
-   Google Android

#### Parameters

-   **command:** The REST API command to have Facebook execute
-   **method:** The request method to call the Facebook REST API, typically GET
    or POST
-   **parameters:** Any parameters required for the request

#### Events

-   **[intel.xdk.facebook.request.response](#requestresponse):** This event is
    fired upon completion of a Facebook API request.

### showAppRequestDialog

This method displays the Facebook application request dialog

```javascript
intel.xdk.facebook.showAppRequestDialog(parameters);
```

#### Description

The Request Dialog sends a Request from one user (the sender) to one or more
users (the recipients). The Request Dialog can be used to send a Request
directly from one user to another or display a Multi Friend Selector Dialog,
allowing the sending user to select multiple recipient users.

#### Platforms

-   Apple iOS
-   Google Android

#### Parameters

-   **parameters:** A JSON object holding the appropriate parameters for the
    request as specified in Facebook's documentation

#### Events

-   **[intel.xdk.facebook.dialog.complete](#dialogcomplete):** This event is
    fired upon completion of the dialog request.
-   **[intel.xdk.facebook.dialog.fail](#dialogfail):** This event is fired if
    Facebook attempted to log the user in during a dialog request, but the login failed.

#### Example

```javascript
document.addEventListener("intel.xdk.facebook.dialog.complete",function(e) {
        console.log("Permissions Request Returned");
        if (e.success == true) {
                console.log("News feed updated successfully");
        } else { console.log("permissions request failed"); }
},false);
var objParameters = {"to":"USER_ID_HERE","message":"My Awesome 
    Message","title":"A title for this dialog would go here"}
intel.xdk.facebook.showAppRequestDialog(objParameters);
```

### showNewsFeedDialog

This method displays the Facebook news feed submission dialog

```javascript
intel.xdk.facebook.showNewsFeedDialog(parameters);
```

#### Platforms

-   Apple iOS
-   Google Android

#### Events

-   **[intel.xdk.facebook.dialog.complete](#dialogcomplete):** This event is
    fired upon completion of the dialog request.
-   **[intel.xdk.facebook.dialog.fail](#dialogfail):** This event is fired if
    Facebook attempted to log the user in during a dialog request, but the login
    failed.

#### Example

```javascript
//This allows the application to post to Facebook for the logged in user
document.addEventListener("intel.xdk.facebook.dialog.complete",function(e) {
    console.log("News Feed Event Returned");
    if (e.success == true) {
        console.log("News feed updated successfully");
    }
},false);
var objParameters = {
    "picture":"http://fbrell.com/f8.jpg",
    "name":"Facebook Dialog",
    "caption":"This is my caption",
    "description":"Using Dialogs to interact with users.",
    "link":"http://xdk.intel.com"
}
intel.xdk.facebook.showNewsFeedDialog(objParameters);
```

Events
------

### busy

Fired if a second Facebook request is made while a previous request is still
pending

### dialog.complete

This event is fired in response to a news feed or app request dialog

#### Description

Returns an event object with the properties success and data

### dialog.fail

This event is fired if Facebook tries to log the user on during a dialog
request, but the login fails

### login

Fired after logging a user into Facebook

#### Description

Returns an event object with the properties success and data

### logout

Fired after logging a user out of Facebook

#### Description

Returns an event object with the properties success and data

### request.response

Fired in response to a Facebook API request

#### Description

Returns an event object with the properties success and data

### session.invalidate

Fired if the Facebook session is invalidated

