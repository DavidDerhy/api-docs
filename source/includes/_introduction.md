# Getting Started

To participate in developer program of Fidor Bank AG you have to be a customer of Fidor Bank AG and have an account at [https://fidor.de](https://fidor.de) or [https://fidorbank.uk](https://fidorbank.uk)

Every full KYC Fidor Bank customer can become a developer by agreeing to the [developer's agreement](https://apm.fidor.de/developer/terms_of_services/current). 

You need to have a full account at Fidor in order to pay for services provided by Fidor TecS AG related to usage of the Fidor APIs. 

The developer tools: [API-Browser](https://developer.fidortecs.com/api-browser/), [fidor_schema](https://github.com/fidor/fidor_schema), [fidor_starter_kits](https://github.com/fidor/fidor_starter_kits), [application manager](https://apm.fidor.de/), sandbox environment and [developer community](https://developer.fidor.de/)  are provided free of charge. You only pay for API usage.

To start developing against Fidor's API you have to create your App first. In order to do so, please login with your Fidor bank account's credentials into the [application manager](https://apm.fidor.de/), accept the developer agreement and add new App.

## Creating an application

Creating a new application is really easy. Visit the [Application Manager](https://apm.fidor.de/) page, login with your banking credentials and accept the developer agreement. After that you can create as many applications as you want.

For every new application a dedicated sandbox environment is created. The sandbox comes with some test data, so that you can start developing your application against the sandbox immediately.

A sandbox user is created along the way. A sandbox user is required during the authentication against the sandbox.

Our sandbox environment offers you the same functionality as the API. So once developed and tested against the sandbox environment, you application will be production ready.

## Approval Process

In most cases the application has to be reviewed before it is allowed to access the live system. However, applications that meet the "Self-API" conditions can go live immediately. 

During the beta phase all applications that are able to submit payments, will have to undergo our review.

## OAuth
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

The Fidor API allows you to retrieve information from customer's Fidor accounts. The extent of access your app is allowed, is configured when you are setting up your application in the App Manager.

In order to access the account, the account holder needs to authorize your app to allow it to access their account. This is achieved
using [OAuth 2](https://tools.ietf.org/html/rfc6749). 

From the user's perspective this works as follows:

  - the user is redirected to Fidor and, in case they are not already logged in, is asked to enter username and password

  - in case the user has not previously authorized your app, the user is displayed the list of permissions your application is requesting and asked to confirm that the app is allowed to access their account.

  - finally, the user is redirected to your app which will be allowed to make API calls

From the perspective of the application, the OAuth2 Authorization Code Grant flow needs to be implemented:

  - the user is redirected via http redirect to the request authorization endpoint by your app:

      `/oauth/authorize?redirect_uri=<redirect_url>&client_id=<client_id>&state=<state>&response_type=code`
                  
  - you will need to provide the following values:

    - `redirect_url` : the url on your server the client will be redirected to once the authorization has been completed successfully or falied. This needs to be one of the redirect urls configured for your app in the app manager (see below)

    - `client_id` : the client_id that has been assigned to your app (see below)

    - `state` : a random state value that will be returned to you, see [OAUTH2 sec 4.1.1](https://tools.ietf.org/html/rfc6749#section-4.1.1)

  - Once the Fidor server has authenticated the user and received authorization, it will redirect the user back to the `redirect_url`
    you provided. Attached to this redirect url will be the provided `state` parameter and a `code` parameter.

  - In case authorization of your app was not performed, you will receive an error response from the server as described in
    [OAuth2 sec 4.1.2.1](https://tools.ietf.org/html/rfc6749#section-4.1.2.1)

  - You MUST check the state parameter to ensure it contains the same value you sent to the  authorize endpoint. If not, you must discontinue processing at this point and should contact Fidor as this is likely an attempt to breach security.

  - Extract the `code` parameter from the response and send it to the Fidor server's Access Token Request endpoint `oauth/token` as described in [OAuth2 sec 4.1.3](https://tools.ietf.org/html/rfc6749#section-4.1.3) you will need to supply the following parameters in the body of a `POST` request, using `application/x-www-form-urlencoded` encoding:

      - `grant_type` : "authorization_code"
      - `code` : the code you received via http redirect, see above
      - `client_id` : the client_id that has been assigned to your app (see below)
      - `client_secret` : the client_secret that has been assigned to your app (see below)
      - `redirect_url` : the same redirect url provided in the authorization request (see above)

  - The server will return either an error (described in [OAuth2 sec 5.2](https://tools.ietf.org/html/rfc6749#section-5.2) or a json
    response containing the `access_token` and information about it.

  - The access token will be of type ["Bearer"](https://tools.ietf.org/html/rfc6750) and needs to be included in the http "Authorization" header of subsequent requests to the API (see [Bearer Token Usage](https://tools.ietf.org/html/rfc6750#section-2.1))

The required parameters (`client_id`, `client_secret` and the `redirect_url`) are available in the App Manager:

Setting | Value
--------- | -----------
Client ID | 84b199c5b4cd9605
Client Secret | 5af2806fd4ee8521250b68d0a6ab1e55
App URL | http://localhost:4567
Allowed IP Addresses | 
Callback URLs | http://localhost:4567
Terms of service URL | http://myapp.com/tos
Customer Service URL | http://myapp.com/customer
Customer Service Email | likeaboss@myapp.com


<!-- ![app_manager](https://github.fidor.de/becker/mobile_oauth/raw/master/oauth_desc/app_manager.png) -->


## Headers

To successfully communicate with Fidor API, you have to provide 3 headers:

Header | Value
--------- | -----------
X-Fidor-API-Token | ca48897797c9275d75e2d7a5bc778721
Accept | application/vnd.fidor.de; version=1,text/json
Content-Type | application/json

<aside class="notice">
  You must replace `ca48897797c9275d75e2d7a5bc778721` with your personal API token.
</aside>

## Starter Kits
Fidor Starter Kits have been created with an idea to help you developing your app immediately. When you create an app in the Application Manager then you immediately getting access to already configured example apps. Until now we provide example apps for following programming languages/frameworks:

- ruby/sinatra
- php (plain)
- java/servlet
- Node.js
- Go

[Our starter kits](https://github.com/fidor/fidor_starter_kits) are also available as a ruby gem on github.

##API Logs
For you to being able to see what your app is doing, we offer you the ability to see and analyze the log files directly in the App Manager. Every app you create gets an "API Logs" view, where you see the requests your app is doing, the traffic your app is creating etc.

##Team Management
To help you develop your apps faster, we've built the team management functionality directly into the app manager. To invite your fellow developers just open the team management view and start typing the email address or the name of the developer you would like to invite to your team. When the developer with provided email address already has an account, he can start help you with your app immediately. In other case the invited developer will be notified by an email. 

##Statistics
We will provide you with statistics for your app in the near future. To give you a little bit more of the insight, some of the statistics might be:

* Overall number of installs
* Number of 'active' users
* Number of new installs in the last 30 days/7 days
* Number of 'active' users in the last 30 days/7 days
* Overall number of requests
* Number of requests per endpoint/app function/user
 *  e.g. number of transactions - internal/credit card etc.
 *  e.g. number of account checks (get transactions)

##Approval Process
We distinguish between two types of applications:

* So called - "self-serve" applications - the ones you created for yourself to access your own account. This kind of applications will automatically get approved. You will get a notification about the your application being available in live environment and can start using it immediately
* The applications that may need an access to a foreign account. This type of applications will have to undergo an approval process. Before your application will go into the approval process, you will have to sign an additional agreement/contact. (Stefan Weiß  do we have something like this already?) You will get notified in every step of the approval process. 