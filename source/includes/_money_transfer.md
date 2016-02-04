#Money Transfer
We differentiate between different types of money transfers. In general we distinguish between the Fidor internal transfers (Fidor to Fidor money transfer) and external transfers (SEPA, GMT, FPS, BACS etc.).

##Internal Transfer - Fidor to Fidor
> Request body

```json
{
  "account_id" : "37635844",
  "receiver": "tracy@gmail.com",
  "external_uid" : "0f25a5f8f",
  "amount" : 1500,
  "subject" : "Lunch, Monday. Thank you"
}
```

> Response body

```
{
  "id": "940879",
  "subject": "Lunch, Monday. Thank you",
  "account_id": "37635844",
  "user_id": "7027",
  "receiver": "tracy@gmail.com",
  "amount": 1500,
  "state": "pending_receiver",
  "created_at": "2015-08-27T21:33:20Z",
  "updated_at": "2015-08-27T21:33:20Z",
  "currency": "EUR",
  "transaction_id": "940879",
  "external_uid": "0f25a5f8"
}
```

> If you try to send a transfer twice

```
{
  "code": 409,
  "errors": [
  {
    "field": "external_uid",
    "message": "must be unique"
  }
  ],
  "message": "An order with this external_uid has already been placed"
}
```

Fidor offers you the possibility to transfer money to other Fidor users without even knowing their account number ("Freunden Geld senden"). To transfer the money you can use one of the following:

- nickname
- email address
- mobile phone number
- twitter nickname
- account identification

You can also initiate an internal transfer to the person, who doesn't yet have an account at Fidor bank. In this case the amount will be blocked (see `/preauths`) and the recipient will be notified about the transferred amount and that they can open an account at Fidor bank to collect the money. The amount will be blocked for 14 days. After that time,  the money will be credited back to the sender's account.

####POST Parameter
Parameter | Description | Format | Mandatory
--------- | ----------- | ----------- | -----------
account_id | Account identifier of the sender (`/accounts/id`)| String | x
receiver | Recipient of the transfer. Possible values are: Fidor nickname, Fidor account identifier, twitter nickname, email address, mobile phone number | String | x
external_uid | Unique ID given by the creator of the transfer. In case a uid is reused for a transfer, it is not executed, this mechanism can be used to prevent double bookings in case of network failure or similar event where transfer status is unknown | String | x
amount | Amount of money you would like to send in account currency, in minor units, e.g. 1 EUR is represented as 100. Must be greater than 0 e.g. at least one cent in EUR | Integer | x
subject | Subject of the transfer| String |

#### Response Parameter
Parameter | Description | Format
--------- | ----------- | -----------
id | ID of transfer | Integer
subject | Subject of the transfer as stated | String
account_id | Account identifier of the sender as stated | String
user_id | Customer ID of the sender | Integer
receiver | Recipient of the transfer as stated | String
amount | Amount as stated | Integer
state | State of transfer | String
created_at | Timestamp of creation | String
updated_at | Timestamp of last update | String
currency | Currency of transfer | String
transaction_id | ID of the corresponding transaction | Integer
external_uid | Unique external ID as stated  | String

##### Transfer states
State | Description
--------- | -----------
success | Transfer was successful
pending_receiver | Transfer needs to be collected by receiver
expired | Transfer expired and the money was returned


### HTTP Request
`GET https://api.fidor.de/internal_transfers`  <sub>index</sub>

`GET https://api.fidor.de/internal_transfers/{id}`  <sub>self</sub>

`POST https://api.fidor.de/internal_transfers`  <sub>create</sub>

### Example:
`POST https://api.fidor.de/internal_transfers`

Request on `/internal_transfers` with the method POST.

##SEPA Credit Transfer
> Request body

```json
{
  "account_id" : "123456789",
  "external_uid" : "666",
  "remote_iban": "AT131490022010010999",
  "remote_bic": "SPADATW1XXX",
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
  "remote_bic": "SPADATW1XXX",
  "state": "success",
  "created_at": "2015-03-04T13:47:48+01:00",
  "updated_at": "2015-03-04T13:47:48+01:00",
  "currency": "EUR",
  "transaction_id": "840",
  "external_uid": "666"
}
```

> If you try to send a transfer twice

```
{
  "code": 409,
  "errors": [
  {
    "field": "external_uid",
    "message": "must be unique"
  }
  ],
  "message": "An order with this external_uid has already been placed"
}
```

To transfer money to any country participating in the SEPA (Single Euro Payments Area) initiative use the `sepa_credit_transfers` endpoint. If you want to send money to another bank account in Germany, you don't even have to provide the BIC. IBAN is enough in that case.

Parameter | Description | Format | Mandatory
--------- | ----------- | ----------- | -----------
account_id | Account identifier of the sender (`/accounts/id`)| String | x
external_uid | Unique ID given by the creator of the transfer. In case a uid is reused for a transfer, it is not executed, this mechanism can be used to prevent double bookings in case of network failure or similar event where transfer status is unknown | String | x
remote_iban | IBAN of the recipient's bank account | String | x
remote_bic | BIC of the recipient's bank account. Optional for transfers between two German bank accounts | String (11 characters!) |
remote_name | Recipient's full name | String | x
amount | Amount of money you would like to send in in account currency, in minor units, e.g. 1EUR is represented as 100. | Integer | x
currency |Currency of Account or Amount. ISO 4217 alpha-3 - 3 letter upcase e.g EUR | String (enum) |
subject | Subject of the transfer | String |
created_at | Creation date-time, never changes | String (date-time) ISO 8601 Date-Time |
updated_at | Last update date-time | String (date-time) ISO 8601 Date-Time |


### HTTP Request
`GET https://api.fidor.de/sepa_credit_transfers`  <sub>index</sub>

`GET https://api.fidor.de/sepa_credit_transfers/{id}`  <sub>self</sub>

`POST https://api.fidor.de/sepa_credit_transfers`  <sub>create</sub>



##Batch Transfer

> GET https://api.fidor.de/batch_transfers

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
                    "remote_bic": "",
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
                    "remote_bic": "",
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

`GET https://api.fidor.de/batch_transfers`  <sub>index</sub>

`GET https://api.fidor.de/batch_transfers/{id}`  <sub>self</sub>

`POST https://api.fidor.de/batch_transfers`  <sub>create</sub>
