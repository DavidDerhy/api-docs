#Approval and Operation
As soon you are happy with testing your application you can switch from "test-mode" to "live-mode". Then your application with work with real accounts, real data and real money. 

##Prerequisites
Only developers that went trough the complete bank account registration process and have a bank account (retail or business) with "full KYC status" can start the approval process for their applications. Please (re-)read the section 2 of [developer agreement](https://apm.fidor.de/developer/terms_of_services/current) for additional prerequisites.

Note that only the developer that created (registered) an application in the first place (application owner) can submit it for approval.

##Self Approval (soon)
If your application uses only a small set of functions (scope) and is set to access only your own account it may not need to be approved by Fidor. The exact scope of these "Self-API applications" is shown in the application manager.
This kind of applications will automatically get approved once you submit it. You will get a notification about your application being available in live environment and can start using it immediately.

##Manual Approval Process
Applications that do not meet the conditions for self-approval, in particular if you need additional services (e.g. SEPA direct debit) or require access to other peoples data or accounts will go through a thorough manual review and approval process. 
In most cases you will have to sign additional contracts before your application will be approved and go life. If you are in doubt, please contact our sales team well enough in advance.

##Statistics
We will provide you with statistics for your application in the near future.

##Check API Logs (soon)
For you to being able to see what your application is doing, we offer you the ability to see and analyze the log files directly in the Application Manager. Every application you create gets an "API Logs" view, where you see the requests your application is doing, the traffic your application is creating etc.

## Changing Scope of Your Application
Once your application is live changing the scope will result to all tokens being revoked. That means that all people that use your application have to go through the rights-granting process (OAuth) again, the next they use the application.

## Revoking And Deleting Your Application
The developer that submitted the application can revoke and or delete the application. Applications that have been revoked must go through the approval process again if you want to put it back live again.

## User view: Manage Installed Applications (soon)
All bank customers can see what applications they "installed" or "subscribed to" and get a detailed view on the rights they have granted to those applications. On the same page customers can "uninstall" applications and thus revoke rights.