# Getting Started
Welcome to the Fidor APIs, your direct access to the functions and services of the world's most innovative Bank: Fidor Bank.
To get started you just need a (free) Fidor Account.

## Create a Fidor Account
If you already have a Fidor bank account just follow the instructions on [how to create an application](#create-an-application).

If you do not have a Fidor account yet, or choose to create a separate account for development visit Fidor Bank's webpage [Fidor DE](https://www.fidor.de/registrierung) and register for an account. Registering for just a Privatkunde/retail account is sufficient for testing the API.

After verification of your email address, you are asked to enter a mobile phone number in order to receive a confirmation SMS. **If you are from outside of Germany you might have problems with receiving SMS, so we suggest that you skip this step** and go directly to the [Application Manager](https://apm.fidor.de/developer/apps). Moreover: To test the API you do not need to go through the complete KYC process.

*Please do not confuse the* Fidor Account *with the* Developer Community Account *you need if you want to take part in the discussion in [Developer Community Forums](https://developer.fidor.de/community/).*

## Create an Application
To start using Fidor's API you have to create (register) your application. In order to do so, please login with your Fidor bank account's credentials into the [Application Manager](https://apm.fidor.de/) and select "Add New App". If you log in for the first time you have to accept the Developer Agreement first.

After that you can create as many applications as you want. 

###Sandbox
For every new application a dedicated test environment (sandbox) is created. The sandbox comes with some test data, and a dedicated sandbox user login. 

Our sandbox offers you the same functionality as the live API. So  you can start developing your application without any risk of loosing money or unintentionally changing data.

### Using Starter Kits
Fidor Starter Kits have been created with an idea to help you developing your application immediately. When you create an application in the Application Manager then you immediately getting access to already configured example application. Until now we provide example application for following programming languages/frameworks:

- ruby/sinatra
- php (plain)
- java/servlet
- Node.js
- Go

[Our starter kits](https://github.com/fidor/fidor_starter_kits) are also available as a ruby gem on github.
