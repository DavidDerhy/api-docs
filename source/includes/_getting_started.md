# Getting Started
Welcome to the Fidor APIs, your direct access to the functions and services of the world's most innovative Bank: Fidor Bank.
To get started all you need is a (free) Fidor Community Account.

## Create a Fidor Account
If you already have a Fidor bank account just follow the instructions on [how to create an application](#create-an-application).

If you do not have a Fidor account yet or would like to create a separate account for development visit Fidor Bank's webpage [Fidor DE](https://www.fidor.de/register) and register. Registering for just a "Fidor Community Account" is sufficient for testing the API.

You will have to provide some personal information, a valid e-mail address and a mobile number (to send you a confirmation SMS). Please follow the [illustrated step-by-step guide here](https://developer.fidor.de/kb/create-an-account/).

You may extend your Fidor Account to a full blown bank account now, but you do not need to. You can do that at any time later.

Once you created your account you can go to the [Application Manager](https://apm.fidor.de/). 

*Please do not confuse the* Fidor Community Account *with the* Developer Community Account *you need if you want to take part in the discussion in [Developer Community Forums](https://developer.fidor.de/community/).*

## Add an Application (to use the API)
In order to use the API, you have to tell Fidor the name, purpose and some technical information of the your application that will connect to the Fidor API. 

Please log in to the [Application Manager](https://apm.fidor.de/) with your Fidor account credentials and select "New App". If you log in for the first time you have to accept the Terms of Service first.

You can add as many applications as you like. 

###Sandbox
Every application is provided with a dedicated test environment (sandbox). The sandbox comes with some test data and a dedicated sandbox user login to simulate an account holder. 

Our sandbox offers you the same functionality as the live API, so you can start developing your application without risk of losing money or unintentionally altering data.

### Using Starter Kits
We provide you with a set of Fidor Starter Kits. They have been created to help you understand how the API and the authentication works. You also can use the code samples as a starting point for your own application. 

They work out of the box - you only have to adjust the URL and callback URL to where you put the program. 

Until now we provide example applications for following programming languages/frameworks:

- ruby/sinatra
- php (plain)
- Node.js
- Go

Our starter kits are also available on [github](https://github.com/fidor/fidor_starter_kits) . Please feel free to add your own code and libraries.
