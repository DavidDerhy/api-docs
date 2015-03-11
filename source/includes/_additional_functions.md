#Overdraft
You can see your overdraft history, your current overdraft or request an overdraft for your account if you don't have one yet.

Method    | Endpoint    | Description
--------- | ----------- | -----------
GET | `https://api.fidor.de/overdrafts` | will get you your overdraft history
GET | `https://api.fidor.de/current` | will show you your current overdraft line 
POST | `https://api.fidor.de/overdrafts` | to request a new overdraft
PUT | `https://api.fidor.de/overdrafts/:id` | to deactivate your overdraft


#Short-Term Loan
