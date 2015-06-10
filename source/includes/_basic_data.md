#Basic Data

##User
> GET https://api.fidor.de/users/current

```json
{
    "id": "94070746",
    "email": "kycfull@fidor.de",
    "last_sign_in_at": "2014-10-22T13:44:52+02:00",
    "created_at": "2014-10-10T17:41:58+02:00",
    "updated_at": "2014-10-23T14:49:21+02:00"
}
```

After the authorization you can query the data for the user using your app. You will get only high-level user information. Following fields will be delivered upon requesting the user information:

Parameter | Description | Format
--------- | ----------- | -----------
id   | Unique user identifier | String
email | The user's email address that can also be used as login | String
last_sign_in_at | Last time the user accessed fidor | String (date-time)  ISO 8601 Date-Time
created_at | Creation date-time, never changes | String (date-time) ISO 8601 Date-Time
updated_at | Last update date-time | String (date-time) ISO 8601 Date-Time

### HTTP Request
`GET https://api.fidor.de/users/current` <sub>current user</sub>


##Customer
> GET https://api.fidor.de/customers

```json
{
    "data": [
        {
            "id": "71616244",
            "email": "kycfull@fidor.de",
            "first_name": "John",
            "last_name": "Doe",
            "gender": "m",
            "title": "Herr",
            "nick": "kycfull",
            "maiden_name": null,
            "adr_street": "teststr",
            "adr_street_number": "2",
            "adr_post_code": "50733",
            "adr_city": "Köln",
            "adr_country": "DE",
            "adr_phone": null,
            "adr_mobile": "+49 555 55 55",
            "adr_fax": null,
            "adr_businessphone": null,
            "birthday": "1973-07-02",
            "is_verified": true,
            "nationality": null,
            "marital_status": null,
            "religion": 0,
            "id_card_registration_city": null,
            "id_card_number": null,
            "id_card_valid_until": null,
            "created_at": "2014-10-10T17:41:58+02:00",
            "updated_at": "2015-02-04T04:08:54+01:00",
            "creditor_identifier": ""
        }
    ],
    "collection": {
        "current_page": 1,
        "per_page": 10,
        "total_entries": 1,
        "total_pages": 1
    }
}
```
Customer gives you much more information about the user

Parameter | Description | Format
--------- | ----------- | -----------
id   | Unique customer identifier | String
email | The customer's email address - same as the user's email address | String
first_name | Customer's first name | String
last_name | Customer's last name | String
gender | Customer's gender | String (enum) - Valid Values: "m", "f"
title | Salutation e.g "Mr." or "Mrs." | Enum
nick | Nickname used in community - can be used as login  | String
maiden_name | Customer's maide name | String
adr_street | Street address of the customer | String
adr_street_number | House number | String
adr_post_code | Postcode of the customer | String
adr_city | City customer lives in | String 
adr_country | Country customer lives in | String
adr_phone | Customer's phone number | String
adr_mobile | Customer's mobile phone number | String
adr_fax | Customer's fax number | String
adr_businessphone | Customer's business phone number | String
birthday | Customer's birhday e.g. "1973-07-02" | Date
is_verified | Indicates whether KYC has been performed | Boolean
nationality | Customer's nationality - Country code as defined in ISO3166 alpha2. e.g "DE", "GB" | String
marital_status | Customer's marital status | Integer (enum) - 1: single, 2: married, 3: widowed, 4: divorced, 5: seperated, 6: de facto
religion | Customer's religion - denomination for tax reasons. 0: no information, 1: no denomination, 2: Protestant, 3: Roman-Catholic, 4-18: Other denominations| Integer (enum) - Valid Values "0", "1", "2", "3", "4", "5", "6", "8", "9", "10", "11", "12", "13", "14", "15", "16", "17", 18"
id_card_registration_city | The | String
id_card_number | The number of customer's identity document | 
id_card_valid_until | The expiration date of customer's identity document| String (date)
created_at | Creation date-time, never changes. ISO 8601 Date-Time e.g. "2014-10-10T17:41:58+02:00" | Datetime 
updated_at | Last update date-time. ISO 8601 Date-Time e.g. "2015-02-04T04:08:54+01:00" | Datetime
creditor_identifier | Creditor Identifier ID set if the customer wants to create direct debits | String

## Account

> GET https://api.fidor.de/accounts

```json
{
    "data": [
                {
                    "id": "71616244",
                    "account_number": "9510260101",
                    "iban": "DE32700222009510260101",
                    "balance": 264398,
                    "balance_available": 264398,
                    "preauth_amount": 0,
                    "cash_flow_per_year": 0,
                    "is_debit_note_enabled": null,
                    "is_trusted": null,
                    "is_locked": false,
                    "currency": "EUR",
                    "overdraft": 0,
                    "created_at": "2014-10-10T17:41:58+02:00",
                    "updated_at": "2015-02-13T04:08:53+01:00",
                    "customers": [
                    {
                      "id": "71616244",
                      "email": "kycfull@fidor.de",
                      "first_name": "John",
                      "last_name": "Doe",
                      "gender": null,
                      "title": "Herr",
                      "nick": "kycfull",
                      "maiden_name": null,
                      "adr_street": "teststr",
                      "adr_street_number": "2",
                      "adr_post_code": "50733",
                      "adr_city": "Köln",
                      "adr_country": "DE",
                      "adr_phone": null,
                      "adr_mobile": null,
                      "adr_fax": null,
                      "adr_businessphone": null,
                      "birthday": "1973-07-02",
                      "is_verified": true,
                      "nationality": null,
                      "marital_status": null,
                      "religion": 0,
                      "id_card_registration_city": null,
                      "id_card_number": null,
                      "id_card_valid_until": null,
                      "created_at": "2014-10-10T17:41:58+02:00",
                      "updated_at": "2015-02-13T04:08:53+01:00",
                      "creditor_identifier": ""
                    }
                 ]
              }
          ],
    "collection": {
        "current_page": 1,
        "per_page": 10,
        "total_entries": 1,
        "total_pages": 1
    }
}
```

Parameter | Description | Format
--------- | ----------- | -----------
id   | Unique account identifier | String
account_number | The bank account number | String
iban | IBAN | String
balance | Account balance in minor units | Integer
balance_available | Available account balance in minor units | Integer
preauth_amount | Amount available for pre-authorization in minor units | Integer
cash_flow_per_year | Amount available for yearly cash flow in minor units. This is the limit of funds an account holder has at their disposal without fulfilling Germany KYC requirements | Integer
is_debit_note_enabled | Whether this account is authorized to initiate direct debit transactions | Boolean
is_trusted | Indicates if this is an escrow account | Boolean
is_locked | Indicates whether the account is locked | Boolean
currency | Currency of Account or Amount. ISO 4217 alpha-3 - 3 letter upcase e.g EUR | String (enum)
overdraft | Available account overdraft in minor units | Integer
created_at | Creation date-time, never changes. ISO 8601 Date-Time e.g. "2014-10-10T17:41:58+02:00" | Datetime 
updated_at | Last update date-time. ISO 8601 Date-Time e.g. "2015-02-04T04:08:54+01:00" | Datetime

For your convenience we also include the information about the customers of the account in a response.


### HTTP Requests

`GET https://api.fidor.de/accounts` 
<sub>index</sub>

`GET https://api.fidor.de/accounts/{id}` 
<sub>self</sub>

`GET https://api.fidor.de/accounts/{id}/transactions` 
<sub>account transaction</sub>

`GET https://api.fidor.de/accounts/{id}/internal_transfers`
<sub>See also: [Internal_transfers](#internal-transfer)</sub>

`GET https://api.fidor.de/accounts/{id}/sepa_credit_transfers`
<sub>account SEPA credit transfers</sub>

`GET https://api.fidor.de/accounts/{id}/sepa_direct_debits`
<sub>See also: [SEPA direct debit](#sepa-direct-debit)</sub>

## Transactions
> GET https://api.fidor.de/transactions/:id

```json
{
  "id": "666",
  "account_id": "66666666",
  "transaction_type": "fidor_payout",
  "subject": "\"Send money to Friends\" recipient: Walter White: Heisenberg",
  "amount": -100000,
  "booking_code": "725",
  "booking_date": 2015-03-01,
  "value_date": "2015-03-02",
  "return_transaction_id": "",
  "created_at": "2015-03-02T13:58:59+01:00",
  "updated_at": "2015-03-02T13:58:59+01:00",
  "currency": "EUR",
  "transaction_type_details": {
    "internal_transfer_id": "666",
    "remote_account_id": "66666666",
    "remote_bic": "",
    "remote_iban": "",
    "remote_name": "",
    "remote_nick": "Heisenberg",
    "remote_subject": "Thanks for nothing"
}
```

Parameter | Description | Format
--------- | ----------- | -----------
id   | Unique customer identifier | String
account_id | Fidor account the transaction belongs to | String
transaction_type | Type of the transaction | String (enum)
subject | Transfer subject (reference) | String
amount | The transferred amount in account currency, in minor units, e.g. 1EUR is represented as 100. Can be negative e.g. if something was withdrawn from an account. | Integer
booking_code | Accounting transaction code in the central banking system | String
booking_date | Date the transaction was booked. ISO 8601 Date | String (date)
value_date | Date the amount was credited to the account. ISO 8601 Date | String (date)
return_transaction_id | If the transaction was marked for return, this references the new return transaction | String
created_at | Creation date-time, never changes. ISO 8601 Date-Time e.g. "2014-10-10T17:41:58+02:00" | Datetime 
updated_at | Last update date-time. ISO 8601 Date-Time e.g. "2015-02-04T04:08:54+01:00" | Datetime
currency | Currency of Account or Amount. ISO 4217 alpha-3 - 3 letter upcase e.g EUR | String (enum)
transaction_type_details | Details specific to this transaction type are collected here | 

##Transaction Type Details
Different transaction type can have different details that are specific for this particular type of the transaction. Some of the transaction types don't have any specific attributes. In that case the `transaction_type_details` object stays empty.

Let's take closer look at the transaction types Fidor supports right now.

###Internal Transfer
Details of the `internal_transfer` object contain extensive information about the transaction's initiator.

####fidor_payin

> fidor_payin

```json
"transaction_type_details": {
  "internal_transfer_id": "666",
  "remote_account_id": "66666666",
  "remote_bic": "FDDODEMMXXX",
  "remote_iban": "DE73700222000066666666",
  "remote_name": "Walther White",
  "remote_nick": "Heisenberg",
  "remote_subject": "Thanks for nothing"
}
```

In case of fidor_payin you will get more information about the sender of the payment.

Parameter | Description | Format
--------- | ----------- | -----------
internal_transfer_id  | Unique account identifier of the transfer transaction belongs to. Refunded transactions have no id | String
remote_account_id     | Unique account identifier of the payment's sender | String
remote_bic            | BIC of the transaction's sender | String
remote_iban           | IBAN of the transaction's sender | String
remote_name           | Full name of the transaction's sender | String
remote_nick           | Community nickname of the transaction's sender | String
remote_subject        | Subject of the transaction | String

####fidor_payout

> fidor_payout

```json
"transaction_type_details": {
  "internal_transfer_id": "666",
  "remote_account_id": "66666666",
  "remote_bic": "",
  "remote_iban": "",
  "remote_name": "",
  "remote_nick": "Heisenberg",
  "remote_subject": "Thanks for nothing"
}
```

`fidor_payout` has same structure but less details because of legal reasons.

Parameter | Description | Format
--------- | ----------- | -----------
internal_transfer_id  | Unique account identifier of the transfer transaction belongs to. Refunded transactions have no id | String
remote_account_id     | Unique account identifier of the payment's sender | String
remote_bic            | empty | String
remote_iban           | empty | String
remote_name           | empty | String
remote_nick           | Community nickname of the transaction's sender | String
remote_subject        | Subject of the transaction | String

###Sepa Credit Transfer
Details of the `sepa_credit_transfer` object contain extensive information about the transaction's initiator or receiver. We differentiate between the incoming `sepa_payin` and outgoing `payout` sepa transactions.

####sepa_payin or payout

> sepa_payin, payout

```json
"transaction_type_details": {
  "sepa_credit_transfer_id": "66666666",
  "remote_name": "Walther White",
  "remote_iban": "DE08100100100666666666",
  "remote_bic": "PBNKDEFFXXX"
}
```

Parameter | Description | Format
--------- | ----------- | -----------
sepa_credit_transfer_id  | Unique account identifier of the transfer transaction belongs to. Refunded transactions have no id | String
remote_name           | Full name of the transaction's sender/receiver | String
remote_iban           | IBAN of the transaction's sender/receiver | String
remote_bic            | BIC of the transaction's sender/receiver | String

###Credit Card
Details of the `sepa_credit_transfer` object contain extensive information about the transaction's initiator or receiver. We differentiate between the incoming `sepa_payin` and outgoing `payout` sepa transactions.

####Transaction Types

* creditcard_annual_fee
* creditcard_atm_fee
* creditcard_foreign_exchange_fee
* creditcard_notification_fee
* creditcard_order_cancellation
* creditcard_order_fee
* creditcard_order_withdrawal_fee
* creditcard_payin
* creditcard_payout
* creditcard_preauth
* creditcard_release

> credit_card details

```json
"transaction_type_details": {
  "cc_category": "R",
  "cc_merchant_category": "5411",
  "cc_merchant_name": "Metro Cash & Carry",
  "cc_sequence": 6666666,
  "cc_type": "00"
}
```

Parameter | Description | Format
--------- | ----------- | -----------
cc_category          | CreditCard category | String
cc_merchant_category | Category given by the merchant | String
cc_merchant_name     | CreditCard merchant initiating the transaction | String
cc_sequence          | Sequence links all the credit_card related transactions together | String
cc_type              | CreditCard type | String
