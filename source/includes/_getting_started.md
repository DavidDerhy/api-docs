# Getting Started
Welcome to the Fidor APIs, your direct access to the functions and services of the world's most innovative Bank: Fidor Bank.
To get started you just need a (free) Fidor Community Account.

## Create a Fidor Account
If you already have a Fidor bank account just follow the instructions on [how to create an application](#create-an-application).

If you do not have a Fidor account yet or choose to create a separate account for development visit Fidor Bank's webpage [Fidor DE](https://www.fidor.de/register) and register for an account. Registering for just a "Fidor Community Account" is sufficient for testing the API.

After verification of your email address, you are asked to enter a mobile phone number and will receive a confirmation SMS with an MTan. Once you have entered the MTan you will have created a new Fidor Account. 

You may extend your Fidor Account to a full blown bank account now, but you do not need to. You can do that at any time later.

Once you went through these steps you can go to the [Application Manager](https://apm.fidor.de/) and 

*Please do not confuse the* Fidor Community Account *with the* Developer Community Account *you need if you want to take part in the discussion in [Developer Community Forums](https://developer.fidor.de/community/).*

## Add an Application (to use the API)
In order to use the API, you have to tell Fidor the name, purpose and some technical information of the programm (application) on your side that will connect to the API on our side. 

Please login with your Fidor account credentials to the [Application Manager](https://apm.fidor.de/) and select "New App". If you log in for the first time you have to accept the Terms of Service first.

You can add as many applications as you want. 

###Sandbox
Every application get's a dedicated test environment (sandbox). The sandbox comes with some test data, and a dedicated sandbox user login. 

Our sandbox offers you the same functionality as the live API. So  you can start developing your application without any risk of loosing money or unintentionally altering data.

### Using Starter Kits
We provide you with a set of Fidor Starter Kits. They been created to help you understand how the API and the authentication works. You also can use the code smaples as starting points for your own application. 

They work out of the box - you just have to adjust the URL and callback URL to where you put the program. 

Until now we provide example application for following programming languages/frameworks:

- ruby/sinatra
- php (plain)
- Node.js
- Go

Our starter kits are also [available on github](https://github.com/fidor/fidor_starter_kits) . Please feel free to add your own code and libraries.