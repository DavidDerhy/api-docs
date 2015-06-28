#Approval and Operation
As soon you are happy with testing your application you can switch from "test-mode" to "live-mode". Then your application will work with real accounts, real data and real money.

##Prerequisites
At the moment only developers that went trough the complete bank account registration process and have a bank account (retail or business) with "full KYC status" can start the approval process for their applications. Please (re-)read the section 2 of [ToS](https://apm.fidor.de/developer/terms_of_services/current) for additional prerequisites.

Note that only the developer that created (registered) an application in the first place (application owner) can submit it for approval.

*We will introduce an approval concept soon that differentiates between the developer's account and the full-KYC bank account the application needs access to. If you have such a situation we can to the appropriate setting for you.*

##Approval Process
Most applications must go through a manual approval process, in particular if you need non-AGB services (e.g. SEPA direct debit) or your application needs access to other peoples data. In some cases you will have to sign additional contracts before your application will be approved and go life. If you are in doubt, please contact our sales team well enough in advance.

There are two basic access modes and approve types:

Approval Type | Use Case | Application can access ...
------- | ---- | ---
**Team Accounts** | Only one bank customer may use use the application | ... only one specific account
**All Accounts** | Everybody may use the application to access their bank account | ... all bank accounts

After a Fidor admin reviewed your approval request you will get an e-mail. If the admin requests some change or needs additional information you can re-submit the application as soon as the changes have been made.

###Switch to Production
Once your application have been approved you can switch from sandbox to the production system.

To connect your application to production use the following URLs:

Function | URL
---- | ----
Production API | https://api.fidor.de
Production OAuth | https://aps.fidor.de/oauth

The Application Client ID and Secret stay the same.

##Statistics
We will provide you with statistics for your application in the near future.

##Check API Logs (soon)
For you to being able to see what your application is doing, we offer you the ability to see and analyze the log files directly in the Application Manager. Every application you create gets an "API Logs" view, where you see the requests your application is doing, the traffic your application is creating etc.

## Changing Scope of Your Application
If your application is live changing the scope will result to all tokens being revoked. That means that all people that use your application have to go through the rights-granting process (OAuth) again, the next they use the application.

## Revoking And Deleting Your Application
The developer that submitted the application can revoke and or delete the application. Applications that have been revoked must go through the approval process again if you want to put it back live again.

## User view: Manage Installed Applications
All bank customers can see what applications they "installed" or "subscribed to" and get a detailed view on the rights they have granted to those applications. On the same page customers can "uninstall" applications and thus revoke rights.
