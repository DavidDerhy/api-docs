#Approval and Operation
Once you have successfully tested your application you can switch from "test-mode" to "live-mode". Once approved, your application will work with real accounts, real data and real money.

##Prerequisites
At the moment only developers that went through the complete bank account registration process and have a bank account (retail or business) with "full KYC status" can start the approval process for their applications. Please (re-)read the section 2 of [ToS](https://apm.fidor.de/developer/terms_of_services/current) for additional prerequisites.

Note that only the developer that created (registered) an application initially (application owner) may submit it for approval.

*We will introduce an approval concept soon that differentiates between the developer's account and the full-KYC bank account the application needs access to. If you have such a situation please get in touch with our API support.*

##Approval Process
Most applications must go through a manual approval process, in particular if you require non-AGB services (e.g. SEPA direct debit) or your application needs access to third party accounts. In some cases you will have to sign additional contracts before your application can be approved and go live. If you are in doubt, please contact our sales team.

There are two basic access modes and approval types:

Approval Type | Use Case | Application can access ...
------- | ---- | ---
**Team Accounts** | Only one bank customer may use use the application | ... only one specific account
**All Accounts** | Everybody may use the application to access their bank account | ... all bank accounts

After a Fidor admin reviewed your approval request you will receive an e-mail. If the admin requests changes or requires additional information you can re-submit the application as soon as the changes have been made.

###Switch to Production
Once your application has been approved you can switch from the sandbox to the production environment.

To connect your application to the production environment use the following URLs:

Function | URL
---- | ----
Production API | https://api.fidor.de
Production OAuth | https://apm.fidor.de/oauth

The Application Client ID and Secret stay the same.

####Transport Layer Security (TLS) and Java
The TLS server in the production environment is configured not to allow weak ciphers specifications. Depending on what version of the Java you are using, it may be necessary to install the 'Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy Files' in order to be able to access the system. Please see: http://docs.oracle.com/javase/7/docs/technotes/guides/security/SunProviders.html#importlimits and http://www.oracle.com/technetwork/java/javase/downloads/index.html for more information and download possibilities

##Statistics
We will provide you with statistics for your application in the near future.

##Check API Logs (soon)
In order to monitor your application, we will offer  the ability to see and analyze log files directly in the Application Manager. Every application you create gets an "API Logs" view, where you see your application's requests, the traffic your application is creating etc.

## Changing the Scope of Your Application
If your application is live, changing the scope will result in all issued tokens being revoked. This means that anyone using your application has to go through the rights-granting process (OAuth) again the next they use the application.

## Revoking And Deleting Your Application
The developer that submitted the application can revoke and or delete the application. Applications that have been revoked must go through the approval process again.

## User view: Manage Installed Applications
All bank customers can see what applications they "installed" or "subscribed to" and get a detailed view of the rights they have granted to those applications. On the same page customers can "uninstall" applications and thus revoke rights. You will not be notified of revoked authorizations and must handle resulting errors during API calls.
