#Overdraft
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

Every new fullKYC fidor customer doesn't have an overdraft but can request one. To request an overdraft you have to make a POST request to the /overdrafts endpoint - `POST https://api.fidor.de/overdrafts`

Following scenarios are possible:

- Your account is not overdraftable or was blacklisted
- You're trying to request an overdraft for a wrong account
- An overdraft was granted to you and you get the detailed information about it

Once you were granted an overdraft you have the possibility to get the history of you previous overdrafts or your current overdraft - `GET https://api.fidor.de/overdrafts`


Parameter | Description | Format
--------- | ----------- | -----------


Method    | Endpoint    | Description
--------- | ----------- | -----------
POST | `https://api.fidor.de/overdrafts` | to request a new overdraft
GET | `https://api.fidor.de/overdrafts` | will get you your overdraft history
GET | `https://api.fidor.de/current` | will show you your current overdraft line 
PUT | `https://api.fidor.de/overdrafts/:id` | to deactivate your overdraft


#Short-Term Loan
