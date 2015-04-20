#Develop and Debug

The developer tools [API browser](https://developer.fidortecs.com/api-browser/), [Fidor schema](https://github.com/fidor/fidor_schema), [Fidor starter kits](https://github.com/fidor/fidor_starter_kits), [Application Manager](https://apm.fidor.de/), sandbox environment and [developer community](https://developer.fidor.de/)  are provided free of charge. You only pay for API usage once your application goes live.

##Add and Change Scopes
Fidor APIs provide a wide range of functions and provide access to a wide range of banking services. In the Application Manager you have to decide and define the scope, i.e. the set of functions your application needs in order to work properly.
Here your can also select whether your application only needs access to your own account or if you plan to provide the application to other people, so that the application needs access to their data and accounts.

##Sandbox
For every new application a dedicated sandbox environment (account simulation) is created. The sandbox comes with some test data, so that you can start developing your application against the sandbox immediately.

Along with this simulated bank accounts a sandbox user (with login and password) is created. A sandbox user is required during the authentication against the sandbox. You can see the login credential of this sandbox user when you select the sandbox tab in the Application Manager.

Our sandbox environment offers you the same functionality as the live API. So once developed and tested against the sandbox environment, your application will be production ready.

##Understand OAuth
```
  /ffffff   /ffffff              /ff     /ff
 /ff__  ff /ff__  ff            | ff    | ff
| ff  \ ff| ff  \ ff /ff   /ff /ffffff  | fffffff
| ff  | ff| ffffffff| ff  | ff|_  ff_/  | ff__  ff
| ff  | ff| ff__  ff| ff  | ff  | ff    | ff  \ ff
| ff  | ff| ff  | ff| ff  | ff  | ff /ff| ff  | ff
|  ffffff/| ff  | ff|  ffffff/  |  ffff/| ff  | ff
 \______/ |__/  |__/ \______/    \___/  |__/  |__/
```

The Fidor API offers many functions to retrieve information from your and other people's bank accounts, community accounts and much more. The extent of access your application needs, is configured when you define the *scope* of your application in the Application Manager.

In order to actually access the account and retrieve the requested data, the account holder needs to authorize your application and allow it to access their account. This is achieved
using [OAuth 2](https://tools.ietf.org/html/rfc6749).

###User perspective
From the user's perspective this works as follows:

  - the user is redirected to Fidor and, in case they are not already logged in, is asked to **enter username and password**

  - in case the user has not previously authorized your app, the user is displayed the list of permissions your application is requesting and **asked to confirm that the app is allowed to access their account with the given scope**.

  - finally, the user is redirected to your app which will be allowed to make API calls

###Application perspective
From the perspective of the application, the OAuth2 Authorization Code Grant flow needs to be implemented:

  - the user is redirected via http redirect to the request authorization endpoint by your application:

      `/oauth/authorize?redirect_uri=<redirect_url>&client_id=<client_id>&state=<state>&response_type=code`

  - you will need to provide the following values:

    - `redirect_url` : the url on your server the client will be redirected to once the authorization has been completed successfully or falied. This needs to be one of the redirect urls configured for your application in the Application Manager (see below)

    - `client_id` : the client_id that has been assigned to your app (see below)

    - `state` : a random state value that will be returned to you, see [OAUTH2 sec 4.1.1](https://tools.ietf.org/html/rfc6749#section-4.1.1)

  - Once the Fidor server has authenticated the user and received authorization, it will redirect the user back to the `redirect_url`
    you provided. Attached to this redirect url will be the provided `state` parameter and a `code` parameter.

  - In case authorization of your app was not performed, you will receive an error response from the server as described in
    [OAuth2 sec 4.1.2.1](https://tools.ietf.org/html/rfc6749#section-4.1.2.1)

  - You MUST check the state parameter to ensure it contains the same value you sent to the  authorize endpoint. If not, you must discontinue processing at this point and should contact Fidor as this is likely an attempt to breach security.

  - Extract the `code` parameter from the response and send it to the Fidor server's Access Token Request endpoint `oauth/token` as described in [OAuth2 sec 4.1.3](https://tools.ietf.org/html/rfc6749#section-4.1.3). You will need to supply the following parameters in the body of a `POST` request, using `application/x-www-form-urlencoded` encoding:

      - `grant_type` : "authorization_code"
      - `code` : the code you received via http redirect (see above)
      - `client_id` : the client_id that has been assigned to your app (see below)
      - `client_secret` : the client_secret that has been assigned to your app (see below)
      - `redirect_url` : the same redirect url provided in the authorization request (see above)

  - The server will return either an error (described in [OAuth2 sec 5.2](https://tools.ietf.org/html/rfc6749#section-5.2) or a JSON
    response containing the `access_token` and information about it.

  - The access token will be of type ["Bearer"](https://tools.ietf.org/html/rfc6750) and needs to be included in the http `Authorization` header of subsequent requests to the API (see [Bearer Token Usage](https://tools.ietf.org/html/rfc6750#section-2.1))

The required parameters (`client_id`, `client_secret` and the `redirect_url`) are available in the App Manager:

Setting | Value (example)
--------- | -----------
Client ID | 84b199c5b4cd9605
Client Secret | 5af2806fd4ee8521250b68d0a6ab1e55
App URL | http://localhost:4567
Allowed IP Addresses | 84.32.22.12, 84.32.22.13, 84.32.22,14
Callback URLs | http://localhost:4567
Terms of service URL | http://myapp.com/tos
Customer Service URL | http://myapp.com/customer
Customer Service Email | likeaboss@myapp.com

<aside class="warning">
  <strong>You must never share Client ID and Client Secret with other people.</strong><br/>
  If it happened unintentionally please copy the application (to get a new Client ID and Client Secret), delete the old one and use the copy.
</aside>


<!-- ![app_manager](https://github.fidor.de/becker/mobile_oauth/raw/master/oauth_desc/app_manager.png) -->

##Check Headers
```bash
curl --header "Accept : application/vnd.fidor.de; version=1,text/json" --header "Content-Type : application/json" --header "Authorization : Bearer b26a06e403f9900a0ddd45e5d6a53bf7" https://api.fidor.de/users/current
```

To successfully communicate with Fidor API, you have to provide 3 headers:

Header | Value
--------- | -----------
Authorization | Bearer ca48897797c9275d75e2d7a5bc778721
Accept | application/vnd.fidor.de; version=1,text/json
Content-Type | application/json

<aside class="notice">
  You must replace <strong>ca48897797c9275d75e2d7a5bc778721</strong> with your own API token. You get this API token during the OAuth process when you call our API for the first time (see above).
</aside>

##Throttling (Rate Limits)
> GET https://api.fidor.de/rate_limit

```json
{
    "limit": 60,
    "remaining": 55,
    "reset": 155
}
```

> In case you exceeded the rate limit:

```json
{
    "code": 429,
    "message": "Rate limit exceeded. Please retry later."
}
```

We throttle our APIs by default to ensure maximum performance for all developers.

Rate limiting in version 1 of the API is primarily considered on a per-user basis — or more accurately described, per access token in your control. Rate limits are determined globally for the entire application.

Rate limits in version 1 of the API are divided into 15 minute intervals. In the version 1 of the API rate limit is 60 calls every 15 minutes, but we may adjust that over time.

###Rate Limit Headers
For you to make a better prediction about the API rate limit for your application or user, please consider to use the `X-RateLimit-*` headers.

We provide three context based headers for this purpose:

HTTP Header | Description
--------- | -----------
X-RateLimit-Limit   | The rate limit ceiling for all requests / 15-min window
X-RateLimit-Remaining | The number of requests left for the the 15-min window
X-RateLimit-Reset | The remaining window before the rate limit resets in e.g. UTC epoch seconds

When an application exceeds the rate limit, the Fidor API will return an HTTP 429 “Too Many Requests” response code (HTTP Status Code Documentation).

For your convenience we provide you an endpoint for requesting the current state of your rate limit `GET https://api.fidor.de/rate_limit`


##Pagination
> https://api.fidor.de/transactions?page=2&per_page=100

```
X-LIMIT → 100
X-OFFSET → 100
X-TOTAL → 300
```

We use the header based pagination for navigating through long lists of found entries.

HTTP Header | Description
--------- | -----------
X-LIMIT   | The per page limit of the entries shown. Default value: 50
X-OFFSET | The pagination offset
X-TOTAL | The total number of the entries

To navigate through the pages please use `page=` and `per_page=` URL parameters as stated in an example on the right pane.

##Manage Teams
To help you develop your apps faster, we've built the team management functionality directly into the Application Manager. To invite your fellow developers just open the team management view and start typing the email address or the name of the developer you would like to invite to your team. When the developer with provided email address already has an account, he can start help you with your app immediately. In other case the invited developer will be notified by an email.
