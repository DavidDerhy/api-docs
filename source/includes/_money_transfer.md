#Money Transfer
We differ between different types of money transfers. In generell we distinguish between the Fidor internal transfers (Fidor to Fidor money transfer) and external transfers (SEPA, GMT, FPS, BACS etc.).

##Internal Transfer
> Request body

```json
{
  "account_id" : "71616244",
  "receiver": "kycfull@fidor.de",
  "external_uid" : "1234567",
  "amount" : 1,
  "subject" : "object"
}
```

> Response body

```
{
  "id": "281",
  "subject": "object",
  "account_id": "71616244",
  "user_id": "4",
  "receiver": "71616244",
  "amount": 1,
  "state": "success",
  "created_at": "2015-02-18T15:38:23+01:00",
  "updated_at": "2015-02-18T15:38:25+01:00",
  "currency": "EUR",
  "transaction_id": "281",
  "external_uid": "1"
}
```

Fidor offers you the possibility to transfer money to your fellow Fidor user without even knowing his account number. To transfer the money you can use one of the following:
- nickname
- email address
- mobile phone number
- twitter nickname
- account identification

You can also initiate an internal transfer to the person, who doesn't yet have an account at Fidor bank. In this case the amount will be blocked and the recipient will be notified about the transferred amount and can open an account at Fidor bank to receive the money. The amount will be blocked for 14 days. During this period of time the recipient can create an account at Fidor and the money will be credited to his account. After the expiration of 14 days, the money will be credited back to the sender's account.

Parameter | Description | Format
--------- | ----------- | -----------
account_id | Account identifier of the sender | String
receiver | Recipient of the transfer. Possible values are: Fidor nickname, Fidor account identifier, twitter nickname, email address, mobile phone number | String
external_uid | Unique ID of the creator of the transaction. In case a uid is reused for a transaction, it is not executed, this mechanism can be used to prevent double bookings in case of network failure or similar event where transaction status is unknown | String
amount | Amount of money you would like to send in account currency, in minor units, e.g. 1EUR is represented as 100. Must be greater than 0 e.g. at least one cent in EUR | Integer
subject | Subject of the transaction | String

### HTTP Request
`GET http://api.fidor.de/internal_transfers`  <sub>index</sub>

`GET http://api.fidor.de/internal_transfers/{id}`  <sub>self</sub>

`POST http://api.fidor.de/internal_transfers`  <sub>create</sub>

### Example: 

`POST http://api.fidor.de/internal_transfers`

Request on `/internal_transfers` with the method POST.

##SEPA Credit Transfer
> Request body

```json
{
  "account_id" : "123456789",
  "external_uid" : "666",
  "remote_iban": "AT131490022010010999",
  "remote_bic": "SPADATW1",
  "remote_name" : "Walter White (Heisenberg)",
  "amount" : 100000,
  "subject" : "Here is your dirty money, Walter!"
}
```

> Response body

```
{
  "id": "49",
  "account_id": "123456789",
  "user_id": "4",
  "subject": "Here is your dirty money, Walter!",
  "amount": 100000,
  "remote_name": "Walter White (Heisenberg)",
  "remote_iban": "AT131490022010010999",
  "remote_bic": "SPADATW1",
  "state": "success",
  "created_at": "2015-03-04T13:47:48+01:00",
  "updated_at": "2015-03-04T13:47:48+01:00",
  "currency": "EUR",
  "transaction_id": "840",
  "external_uid": "666"
}
```

To transfer money to any country participating in SEPA (Single Euro Payments Area) initiative you will have to use the `sepa_credit_transfers` endpoint
`POST https://api.fidor.de/sepa_credit_transfers`

If you want to send money to another bank account in Germany, you don't have to provide the BIC - IBAN is enough.

Parameter | Description | Format
--------- | ----------- | -----------
account_id | Account identifier of the sender | String
external_uid | Unique ID of the creator of the transaction. In case a uid is reused for a transaction, it is not executed, this mechanism can be used to prevent double bookings in case of network failure or similar event where transaction status is unknown | String
remote_iban | IBAN of the recipient's bank account | String
remote_bic | BIC of the recipient's bank account. Optional for transfers between two German bank accounts | String
remote_name | Recipient's full name | String
amount | Amount of money you would like to send in in account currency, in minor units, e.g. 1EUR is represented as 100. | Integer
currency |Currency of Account or Amount. ISO 4217 alpha-3 - 3 letter upcase e.g EUR | String (enum)
subject | Subject of the transfer | String
created_at | Creation date-time, never changes | String (date-time) ISO 8601 Date-Time
updated_at | Last update date-time | String (date-time) ISO 8601 Date-Time


##Batch Transfer

> GET http://api.fidor.de/batch_transfers

```
{
    "data": [
        {
            "account_id": "71616244",
            "created_at": "2015-02-03T12:18:45+01:00",
            "external_uid": "216762",
            "id": "2",
            "internal_transfer_errors": [],
            "internal_transfer_ids": [
                "275"
            ],
            "sepa_credit_transfer_errors": [],
            "sepa_credit_transfer_ids": [
                "38"
            ],
            "state": "success",
            "success_url": "",
            "transfers_count": 2,
            "updated_at": "2015-02-03T12:18:45+01:00",
            "user_id": "4",
            "sepa_credit_transfers": [
                {
                    "id": "38",
                    "account_id": "71616244",
                    "user_id": "4",
                    "subject": "object",
                    "amount": 1,
                    "remote_name": "Walter Yoplack",
                    "remote_iban": "DE49140520002640025972",
                    "remote_bic": null,
                    "state": "received",
                    "created_at": "2015-02-03T12:18:42+01:00",
                    "updated_at": "2015-02-03T12:18:42+01:00",
                    "currency": "EUR",
                    "transaction_id": "",
                    "external_uid": "978991"
                }
            ],
            "internal_transfers": [
                {
                    "id": "275",
                    "subject": "object",
                    "account_id": "71616244",
                    "user_id": "4",
                    "receiver": "71616244",
                    "amount": 1,
                    "state": "success",
                    "created_at": "2015-02-03T12:18:42+01:00",
                    "updated_at": "2015-02-03T12:18:44+01:00",
                    "currency": "EUR",
                    "transaction_id": "275",
                    "external_uid": "494972"
                }
            ]
        },
        {
            "account_id": "71616244",
            "created_at": "2015-02-12T14:22:38+01:00",
            "external_uid": "595177",
            "id": "3",
            "internal_transfer_errors": [],
            "internal_transfer_ids": [
                "277"
            ],
            "sepa_credit_transfer_errors": [],
            "sepa_credit_transfer_ids": [
                "40"
            ],
            "state": "success",
            "success_url": "",
            "transfers_count": 2,
            "updated_at": "2015-02-12T14:22:38+01:00",
            "user_id": "4",
            "sepa_credit_transfers": [
                {
                    "id": "40",
                    "account_id": "71616244",
                    "user_id": "4",
                    "subject": "object",
                    "amount": 1,
                    "remote_name": "Walter Yoplack",
                    "remote_iban": "DE49140520002640025972",
                    "remote_bic": null,
                    "state": "success",
                    "created_at": "2015-02-12T14:22:37+01:00",
                    "updated_at": "2015-02-12T14:22:37+01:00",
                    "currency": "EUR",
                    "transaction_id": "800",
                    "external_uid": "835003"
                }
            ],
            "internal_transfers": [
                {
                    "id": "277",
                    "subject": "object",
                    "account_id": "71616244",
                    "user_id": "4",
                    "receiver": "71616244",
                    "amount": 1,
                    "state": "success",
                    "created_at": "2015-02-12T14:22:37+01:00",
                    "updated_at": "2015-02-12T14:22:38+01:00",
                    "currency": "EUR",
                    "transaction_id": "277",
                    "external_uid": "624437"
                }
            ]
        }
    ],
    "collection": {
        "current_page": 1,
        "per_page": 10,
        "total_entries": 4,
        "total_pages": 1
    }
}
```

PREVIEW: A batch transfer can contain multiple transfers which are processed asynchronously. You can include both `internal_transfers` and `sepa_credit_transfers` in your batch.

### HTTP Request

`GET http://api.fidor.de/batch_transfers`  <sub>index</sub>

`GET http://api.fidor.de/batch_transfers/{id}`  <sub>self</sub>

`POST http://api.fidor.de/batch_transfers`  <sub>create</sub>

##Coming Soon - Extended Money Operations

In order to create a SEPA Direct Debit you have to crete a SEPA Mandate first. Not every Fidor customer is eligible for creating the SEPA Mandates. If you are interested in SEPA Direct Debits and do not have a required account for creating SEPA Mandates you will have to contact Fidor first.

### HTTP Request

`GET http://api.fidor.de/sepa_direct_debits` <sub>index</sub>

`GET http://api.fidor.de/sepa_direct_debits/{id}` <sub>self</sub>

`POST http://api.fidor.de/sepa_direct_debits` <sub>create</sub>

###Creating SEPA Mandate

###Creating SEPA Direct Debit

###Batch Direct Debit 

*PREVIEW:* A direct debit batch contains multiple direct debits which are processed asynchronously.

`GET http://api.fidor.de/batch_direct_debits/{id}` 
<sub>self</sub>

`GET http://api.fidor.de/batch_direct_debits` 
<sub>instances</sub>

`POST http://api.fidor.de/batch_direct_debits` 
<sub>create</sub>