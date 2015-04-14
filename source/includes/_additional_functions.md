#Overdraft
Fidor offers its customers to request an overdraft for their accounts. Overdraft is calculated based on your montly income for the last three months and your current account balance. Since your current balance may vary significantly from day to day, we will show you the overdraft line once you requested and received it.

Parameter | Description | Format
--------- | ----------- | -----------
id   | Unique user identifier | String
account_id | Account identifier of the sender | String
line | Overdraft line in account currency, in minor units, e.g. 1EUR is represented as 100. Must be greater than 0 e.g. at least one cent in EUR | Integer
active | An identifier whether the overdraft is active or not | Boolean
currency | Currency of Account or Amount. ISO 4217 alpha-3 - 3 letter upcase e.g EUR | String (enum)
created_at | Creation date-time, never changes | String (date-time) ISO 8601 Date-Time
updated_at | Last update date-time | String (date-time) ISO 8601 Date-Time


Method    | Endpoint    | Description
--------- | ----------- | -----------
POST | `https://api.fidor.de/overdrafts` | To request a new overdraft
GET | `https://api.fidor.de/overdrafts` | To get your overdraft history
GET | `https://api.fidor.de/overdrafts/current` | To get you current overdraft
PUT | `https://api.fidor.de/overdrafts/:id` | To deactivate your overdraft

## Request an overdraft
> POST /overdrafts

```
{
  "account_id" : "7654321"
}
```

> Account not overdraftable or blacklisted

```
{
  "code": 403,
  "message": "Your account is not eligible for overdrafts"
}
```

> Requesting an overdraft for a wrong account

```
{
  "code": 422,
  "errors": [
      {
          "field": "account_id",
          "message": "should be the authenticated account id"
      }
  ]
}
```

> When the request was successfull you will get the information about your overdraft in the response

```
{
  "account_id": "7654321",
  "line": 45000,
  "interest_rate": 2.0,
  "active": true
}
```

Every new fullKYC fidor customer doesn't have an overdraft but can request one. To request an overdraft you have to make a POST request to the /overdrafts endpoint - `POST https://api.fidor.de/overdrafts`

Following scenarios are possible:

- Your account is not overdraftable or was blacklisted
- You're trying to request an overdraft for a wrong account
- An overdraft was granted to you and you get the detailed information about it


Every new fullKYC fidor customer doesn't have an overdraft but can request one. To request an overdraft you have to make a POST request to the /overdrafts endpoint - `POST https://api.fidor.de/overdrafts`

## History of Overdrafts and Current Overdraft
> GET https://api.fidor.de/overdrafts - history of your overdrafts

```
{
  "data" : [
   {
    "account_id" : "7654321",
    "line" : 500,
    "interest_rate" : 4.32,
    "active" : true,
    "created_at" : "2014-12-09T11:34:59+01:00",
    "updated_at" : "2014-12-19T17:41:29+01:00"
   },
   {
    "account_id" : "7654321", 
    "line" : 750,
    "interest_rate" : 4.32,
    "active" : false,
    "created_at" : "2014-12-09T11:34:59+01:00",
    "updated_at" : "2014-12-19T17:41:29+01:00"
   }
  ]
}
```

> GET https://api.fidor.de/overdrafts/current

```
{
  "account_id" : "7654321",
  "line" : 500,
  "interest_rate" : 4.32,
  "active" : true,
  "created_at" : "2014-12-09T11:34:59+01:00",
  "updated_at" : "2014-12-19T17:41:29+01:00"
}
```

> If you don't have an overdraft yet

```
{
  "error" : {
    "code": 404,
    "errors": [],
    "message": "No current overdraft"
  }
}
```

Once you were granted an overdraft you have the possibility to get the history of you previous overdrafts or your current overdraft - `GET https://api.fidor.de/overdrafts`


## Deactivate an Overdraft
> PUT https://api.fidor.de/overdrafts/{:id}

```
{
  "deactivate" : true
}
```

In case you don't need an overdraft anymore you have the possibility to deactivate it. To deactivate an overdraft you have to send a `PUT` request to the overdraft's endpoint `https://api.fidor.de/overdrafts/{:id}` with the request body containing the `"deactivate"` property set to `true`.

#Short-Term Loan

Parameter | Description | Format
--------- | ----------- | -----------
id   | Unique user identifier | String
account_id | Account identifier of the sender | String
loan_amount | Overdraft line in account currency, in minor units, e.g. 1EUR is represented as 100. Must be greater than 0 e.g. at least one cent in EUR | Integer
duration | Short Term Loan's duration | 
fee_amount | Fee for a short term loan in account currency, in minor units, e.g. 1EUR is represented as 100. Must be greater than 0 e.g. at least one cent in EUR| Integer
state | State of the short term loan | String
total_amount | A total amount of a short term loan in minor units | Integer
currency | Currency of Account or Amount. ISO 4217 alpha-3 - 3 letter upcase e.g EUR | String (enum)
active | An identifier whether the overdraft is active or not | Boolean
created_at | Creation date-time, never changes | String (date-time) ISO 8601 Date-Time
updated_at | Last update date-time | String (date-time) ISO 8601 Date-Time


Method    | Endpoint    | Description
--------- | ----------- | -----------
GET | `https://api.fidor.de/short_term_loans/new` | To request details about available short term loan prior to requesting one
POST | `https://api.fidor.de/short_term_loans` | To request a new short term loan
GET | `https://api.fidor.de/short_term_loans` | To get short term loan history
GET | `https://api.fidor.de/short_term_loans/current` | To get the current short term loan
PUT | `https://api.fidor.de/short_term_loans/:id` | To pay back current short term loan

## Get the current short term loan
> GET https://api.fidor.de/short_term_loans/current

```
{
    "id": "6",
    "account_id": "95828151",
    "loan_amount": 10000,
    "duration": 30,
    "redemption_amount": 0,
    "fee_amount": 600,
    "state": "created",
    "redemption_at": "2015-05-02T15:16:21Z",
    "created_at": "2015-04-02T15:16:21Z",
    "updated_at": "2015-04-02T15:16:21Z",
    "currency": "EUR"
}
```

> In case you don't have an active short_term_loan.

```
{
    "code": 404,
    "errors": [],
    "message": "No currently active short term loan found"
}
```


## History of Previously Taken Short Term Loans
> Including currently active one

```
{
  "data" : [
    {
        "id": "6",
        "account_id": "95828151",
        "loan_amount": 10000,
        "duration": 30,
        "redemption_amount": 0,
        "fee_amount": 600,
        "state": "paid",
        "redemption_at": "2015-05-02T15:16:21Z",
        "created_at": "2015-04-02T15:16:21Z",
        "updated_at": "2015-04-02T15:16:21Z",
        "currency": "EUR"
    },
    {
        "id": "7",
        "account_id": "95828151",
        "loan_amount": 10000,
        "duration": 30,
        "redemption_amount": 0,
        "fee_amount": 600,
        "state": "active",
        "redemption_at": "2015-05-02T15:16:21Z",
        "created_at": "2015-04-02T15:16:21Z",
        "updated_at": "2015-04-02T15:16:21Z",
        "currency": "EUR"
    }
  ]
}
```

> In case you don't have any short_term_loans yet.

```
{
    "code": 404, 
    "errors": [], 
    "message": "No currently active short term loan found"
}
```

<aside class="notice">
  If you haven't requested any short_term_loans yet, or requested one but not activated it yet.
</aside>

## New Short Term Loan preview
> GET https://api.fidor.de/short_term_loans/new

```
{
    "account_id": "95828151",
    "loan_amount": 10000,
    "duration": 30,
    "fee_amount": 600,
    "total_amount": 10600,
    "currency": "EUR"
}
```

> If you already have an active short term loan

```
{
    "code": 403,
    "errors": [],
    "message": "You already have an active short term loan. Please pay your loan back before requesting a new one."
}
```

> If your account is not eligible for a short term loan

```
{
    "code": 403,
    "errors": [],
    "message": "Your account is not eligible for a short term loan."
}
```

## Request a Short Term Loan
> POST https://api.fidor.de/short_term_loans

> request body

```
{
  "account_id" : "95828151"
}
```

> response body

```
{
    "id": "6",
    "account_id": "95828151",
    "loan_amount": 10000,
    "duration": 30,
    "redemption_amount": 0,
    "fee_amount": 600,
    "state": "created",
    "redemption_at": "2015-05-02T15:16:21Z",
    "created_at": "2015-04-02T15:16:21Z",
    "updated_at": "2015-04-02T15:16:21Z",
    "currency": "EUR"
}
```

> If there are any errors during request

```
{
  "code": 422,
  "errors": [
      {
          "field": "account_id",
          "message": "should be the authenticated account id"
      }
  ]
}
```

## Pay back current short term loan
> PUT https://api.fidor.de/short_term_loans/:id

> request body

```
{
  "account_id" : 95828151
}
```

> response body

```
// To be defined.
```