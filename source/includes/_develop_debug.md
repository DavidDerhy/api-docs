#Develop and Debug

The developer tools [API browser](https://developer.fidortecs.com/api-browser/), [Fidor schema](https://github.com/fidor/fidor_schema), [Fidor starter kits](https://github.com/fidor/fidor_starter_kits), [Application Manager](https://apm.fidor.de/), sandbox environment and [developer community](https://developer.fidor.de/)  are provided free of charge. You only pay for API usage once you submit your application for approval and it goes live.

##Add and Change Scopes
Fidor APIs provide a wide range of functions and provide access to a wide range of banking services. In the Application Manager you have to decide and define the scope, i.e. the set of functions your application needs in order to work properly.
Here your can also select whether your application only needs access to your own account or if you plan to provide the application to other people, so that the application needs access to their data and accounts.

##Sandbox
For every new application a dedicated sandbox environment (account simulation) is created. The sandbox comes with some test data, so that you can start developing your application against the sandbox immediately.

To connect your application to the sandbox use the following URLs:

Function | URL
---- | ----
Sandbox API | https://aps.fidor.de
Sandbox OAuth | https://aps.fidor.de/oauth

Along with this simulated bank accounts a *sandbox user* (with login and password) is created. A sandbox user is required during the authentication against the sandbox. You can see the login credential of this sandbox user when you select the sandbox tab in the Application Manager.

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

The Fidor API offers many functions to retrieve information from your and other people's bank accounts, community accounts and much more. The extent of access your application needs is configured when you define the *scope* of your application in the Application Manager.

In order to actually access the account and retrieve the requested data, the account holder needs to authorize your application and allow it to access their account. This is achieved
using [OAuth 2](https://tools.ietf.org/html/rfc6749).


### Client facing apps vs. server side connection
Fidor customers have diverse use cases for the Fidor API, ranging from payment automation without any user interaction to situations where consumers will sit in front of the computer every time they use the application. Applications run on PCs, mobile devices, on premise service and in the cloud. All this has implications on time and means of authentication and authorization.

The primarily supported authentication method from Fidor is the 3 legged OAuth2 flow, that is particularly useful for server based client-facing apps and supports 3rd party application providing. The following discussion and documentation is based on that mode.

For applications to simply control your our own bank account via a pure server side connection we soon will provide an improved approval process, OAuth flow and OAuth features. Also mobile applications require special treatment. Please get in contact with us via e-mail or leave a message in the approval submission form.

###Accountholder perspective
From the accountholder's perspective the process works as follows:

- the accountholder using your application is redirected to Fidor. In case they are not already logged in, they are asked to **enter  username and password**
- in case the accountholder has not previously authorized your app, the user is displayed the list of permissions your application is requesting and is **asked to confirm that the app is allowed to access their account with the given scope**.

Afterwards the accountholder is returned to your app which will make calls to the API and display the results.

###Application perspective

From the perspective of your application, the OAuth2 process consists of three steps:

  1. you **redirect** the accountholder's browser to the [request authorization endpoint](#systems) endpoint
  2. the `request authorization endpoint` returns an **Authorization Code** to your server after the accountholder was successfully authenticated
  3. you **retrieve the OAuth bearer token** from Fidor by sending your `client_id` and `client_secret` to the `token` endpoint along with the Authorization Code received in the previous step.

This bearer token is used to authenticate calls to the API. These three steps are elaborated in detail in the following sections:

#### Accountholder Redirect

In order to retrieve an OAuth  token to access an accountholder's account, first instruct your application to redirect the accountholder's browser to the [request authorization endpoint](#systems). Typically, this is done by returning an HTTP status of 3xx and the redirect location in an HTTP `Location` header:

      `/oauth/authorize?redirect_uri=<redirect_uri>&client_id=<client_id>&state=<state>&response_type=code`

you will need to provide the following values:

- `redirect_uri` : the url on your server the client will be redirected to once the authorization has been completed successfully or falied. This needs to be one of the redirect urls configured for your application in the Application Manager (see below)
- `client_id` : the client_id that has been assigned to your app (see below)
- `state` : a random state value that will be returned to you
    
For more details about this step, see [OAUTH2 sec 4.1.1](https://tools.ietf.org/html/rfc6749#section-4.1.1)

#### Extract the Authorization Code

Once the Fidor server has authenticated the user and established authorization, it will redirect the user back to the `redirect_uri`
you provided. Attached to this redirect uri will be the provided `state` parameter and a `code` parameter. For example, you would receive a call from the accountholder's browser to:

  `<redirect_uri>?code=750c9d02197a2d0a2a37b12155b5c3d8&state=<state>`

In this example the value of the Authorization Code is 750c9d02197a2d0a2a37b12155b5c3d8. You MUST check the `state` parameter to ensure it contains the same value you sent to the authorize endpoint. If not, you must discontinue processing at this point and should contact Fidor as this is likely an attempt to breach security!

In case authorization of your app failed, you will receive an [error response](#errors) from the server as described in
[OAuth2 sec 4.1.2.1](https://tools.ietf.org/html/rfc6749#section-4.1.2.1). Authorization typically fails if the accountholder is not willing to allow your app to access their account.

Please refer to [OAuth2 sec 4.1.2](https://tools.ietf.org/html/rfc6749#section-4.1.2) for details concerning this step of the OAuth process.

#### Retrieve the OAuth Token

After you extract the `code` parameter from the server response, it is send via POST to the [Access Token Request endpoint](#systems) `oauth/token` as described in [OAuth2 sec 4.1.3](https://tools.ietf.org/html/rfc6749#section-4.1.3). This request also contains following parameters in the body of a `POST` request, using `application/x-www-form-urlencoded` encoding:

- `grant_type` : "authorization_code"
- `code` : the code you received via http redirect (see above)
- `client_id` : the client_id that has been assigned to your app (see below)
- `redirect_uri` : the same redirect URL provided in the authorization request (see above)

This http request to the `token` endpoint is authenticated using [HTTP Basic Authentication](http://tools.ietf.org/html/rfc1945#section-11.1). The `client_id` and `client_secret` parameters of your app are used as the basic `userid-password` credentials.

### Bearer and Refresh Tokens

The server will either return  an error (described in [OAuth2 sec 5.2](https://tools.ietf.org/html/rfc6749#section-5.2) or a JSON response containing the `access_token`, token information and a `refresh_token`.

The access token will be of type ["Bearer"](https://tools.ietf.org/html/rfc6750) and needs to be included in the http `Authorization` header of subsequent requests to the API (see [Bearer Token Usage](https://tools.ietf.org/html/rfc6750#section-2.1))

Once the `access_token` has expired, the `refresh_token` may be used in order to retrieve a new valid `access_token`. Refresh token requests are addressed to the same endpoint as authorization grant request, but contain the following parameter, as described in [OAuth2 §6](https://tools.ietf.org/html/rfc6749#section-6):

- `grant_type` : "refresh_token"
- `refresh_token` : the refresh token you received in the authorization code flow, above

### Token Revocation

For security reasons, you should revoke a token after you no longer need it to ensure it is not misused. Token Revocation is handled according to [RFC 7009](https://tools.ietf.org/html/rfc7009). It's very simple, just post the token you wish to cancel to the revocation endpoint (`/oauth/revoke`) in a paramter key called `token`. This request needs to be authorized by `client_id` and `client_secret` parameters which are provided via HTTP Basic authentication. Both bearer tokens and refresh tokens can be canceled in the same manner.


### Overview of OAuth Parameters
The required parameters (`client_id`, `client_secret` and the `redirect_uri`) are available in the App Manager:

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

### More examples
The OAuth process is a bit daunting at first and may take some attempts to get right. In case you hvae problems retrieving a token we've provided sample implementations that handle the OAuth process. Have a look at out [Starter Kits](https://github.com/fidor/fidor_starter_kits) to see how OAuth can be implemented.

<aside class="warning">
  <strong>You must never share the Client Secret parameter with other people. This is your applications password.</strong><br/>
  If it happened unintentionally please copy the application (to get a new Client ID and Client Secret), delete the old one and use the copy.
</aside>

##Check Headers
```bash
curl --header "Accept : application/vnd.fidor.de; version=1,text/json" --header "Content-Type : application/json" --header "Authorization : Bearer <your token>" https://api.fidor.de/users/current
```

To successfully communicate with the Fidor API, you must provide 3 headers:

Header | Value
--------- | -----------
Authorization | Bearer ca48897797c9275d75e2d7a5bc778721
Accept | application/vnd.fidor.de; version=1,text/json
Content-Type | application/json

<aside class="notice">
  You must replace <strong>ca48897797c9275d75e2d7a5bc778721</strong> with your personal access_token, the token is generated via the OAuth process described above.
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

Rate limiting in version 1 of the API is primarily considered on a per-user basis — or more accurately, per access token in your control. Rate limits are determined globally for the entire application.

Rate limits in version 1 of the API are divided into 15 minute intervals. In the version 1 of the API rate limit is 60 calls every 15 minutes, but this value may be adjusted at our discretion.

###Rate Limit Headers
For you to make a better prediction about the API rate limit for your application or user, please use the `X-RateLimit-*` headers.

We provide three context based headers for this purpose:

HTTP Header | Description
--------- | -----------
X-RateLimit-Limit   | The rate limit ceiling for all requests / 15-min window
X-RateLimit-Remaining | The number of requests left for the the 15-min window
X-RateLimit-Reset | The remaining window before the rate limit resets in e.g. UTC epoch seconds

When an application exceeds the rate limit, the Fidor API will return an HTTP 429 “Too Many Requests” response code (HTTP Status Code Documentation).

For your convenience we provide an endpoint for requesting the current state of your rate limit `GET https://api.fidor.de/rate_limit`

##Pagination
Not all available data is dumped at once. With pagination you can control the output and navigate through the pages.
> https://api.fidor.de/transactions?page=2&per_page=100
> https://api.fidor.de/preauths?page=1&per_page=20

Parameter | Description
--------- | -----------
per_page   | Per page limit of the entries shown. Default is 10, max is 100
page | The pagination offset

###Collection
Most data you get from the API will have a supplementary  `collection` object to provide information about the pagination and indicate your current place in a collection.

```json
"collection": {
	"current_page": 1,
	"per_page": 10,
	"total_entries": 785,
	"total_pages": 79
}
```

Parameter | Description
--------- | -----------
current_page   | Current page of output
per_page| Number of entries per page
total_entries | Total number of entries found
total_pages | Total number of pages based on the given pagination


##Manage Teams
To help you develop your apps faster, we've built the team management functionality directly into the Application Manager. To invite your fellow developers just open the team management view and start typing the email address or the name of the developer you would like to invite to your team. If the developer with provided email address already has a Fidor account, she can start to help you with your app immediately. In other cases, the invited developer will be notified by email.
