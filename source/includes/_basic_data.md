#Core Resources

##Users
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

After authorization you can query the user data of the authorized account holder. You will only receive high-level user information. The following fields will be delivered upon requesting the user information:

Parameter | Description | Format
--------- | ----------- | -----------
id   | Unique user identifier | String
email | The user's email address that can also be used as login | String
last_sign_in_at | Last time the user accessed fidor | String (date-time)  ISO 8601 Date-Time
created_at | Creation date-time, never changes | String (date-time) ISO 8601 Date-Time
updated_at | Last update date-time | String (date-time) ISO 8601 Date-Time

### HTTP Request
`GET https://api.fidor.de/users/current` <sub>current user</sub>


## Customers
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
Customers gives you all the personal data of the customer

Parameter | Description | Format
--------- | ----------- | -----------
id   | Unique customer identifier | String
email | The customer's email address - same as the user's email address | String
first_name | Customer's first name | String
last_name | Customer's last name | String
gender | Customer's gender | String (enum) - Valid Values: "m", "f"
title | Salutation e.g "Mr." or "Mrs." | Enum
nick | Nickname used in community - can be used as login  | String
maiden_name | Customer's maiden name | String
adr_street | Street address of the customer | String
adr_street_number | House number | String
adr_post_code | Postal code of the customer | String
adr_city | City customer lives in | String
adr_country | Country customer lives in | String
adr_phone | Customer's phone number | String
adr_mobile | Customer's mobile phone number | String
adr_fax | Customer's fax number | String
adr_businessphone | Customer's business phone number | String
birthday | Customer's birthdate e.g. "1973-07-02" | Date
is_verified | Indicates whether KYC has been performed | Boolean
nationality | Customer's nationality - Country code as defined in ISO3166 alpha2. e.g "DE", "GB" | String
marital_status | Customer's marital status | Integer (enum) - 1: single, 2: married, 3: widowed, 4: divorced, 5: separated, 6: de facto
religion | Customer's religion - denomination for tax reasons. 0: no information, 1: no denomination, 2: Protestant, 3: Roman-Catholic, 4-18: Other denominations| Integer (enum) - Valid Values "0", "1", "2", "3", "4", "5", "6", "8", "9", "10", "11", "12", "13", "14", "15", "16", "17", 18"
id_card_registration_city | The | String
id_card_number | The number of customer's identity document |
id_card_valid_until | The expiration date of customer's identity document| String (date)
created_at | Creation date-time, never changes. ISO 8601 Date-Time e.g. "2014-10-10T17:41:58+02:00" | Datetime
updated_at | Last update date-time. ISO 8601 Date-Time e.g. "2015-02-04T04:08:54+01:00" | Datetime

##Create a Customer

### Check MSISDN

> POST https://apm.fidor.de/msisdn/check

```
{
  "msisdn"        : "491666666666",
  "os_type"       : "Android",
  "affiliate_uid" : "1398b666-6666-6666-6666-666666666666"
}
```

> Response

```
Tm7YvR1ttXvSwWAs0t9Kz1Z6dwY6XcrKzH21gBpgyBRisIzn4R2X8DOAlqEV5Z44aXsN8A
```

In order to create a customer object the mobile phone number of the customer you're about to create has to be verified first.

Arguments | Description | Format
--------- | ----------- | -----------
msisdn | Mobile phone Number in MSISDN format | String in <a href='http://www.msisdn.org/' target='_blank'>MSISDN format</a>  
os_type | OS type of the customer's mobile device | String (enum) - "iOS" or "Android"
affiliate_uid | Unique identifier of the affiliate for which customer is created | String (e.g. `"1398b666-6666-6666-6666-666666666666"`)

<aside class="notice">
  As a result of this call you will get a code back that you have to send alongside the `/msisdn/verify` call.
</aside>


### Verify MSISDN

> POST https://apm.fidor.de/msisdn/verify

```
{
  "msisdn" : "491666666666",
  "code"   : "Tm7YvR1ttXvSwWAs0t9Kz1Z6dwY6XcrKzH21gBpgyBRisIzn4R2X8DOAlqEV5Z44aXsN8A"
}
```

> Response

```
rfjjEEwooyf1vYg-5rIu-VZVtIegy9DvsNWSq3pVfDIz9jVmUW3UsPnoARvbbhFFMuBHtg
```

Once you acquired a token for you mobile phone number verification. You have to assemble a verify request as following:

Arguments | Description | Format
--------- | ----------- | -----------
msisdn | Mobile phone Number in MSISDN format | String in <a href='http://www.msisdn.org/' target='_blank'>MSISDN format</a>  
code | Verification code you acquired in the `/msisdn/check` step | String

<aside class="notice">
  As a result of this call you will get a code back that you have to send alongside the `POST /users` call.
</aside>



### Create Customer Object

> POST https://api.fidor.de/customers

```
{
  "affiliate_uid" : "<your affiliate_uid we provide you with>",
  "email":"walther@heisenberg.com",
  "password":"superDuperSecret",
  "adr_mobile":"4917666666666",
  "title":1,
  "first_name":"Walther",
  "additional_first_name" : "Heisenberg",
  "last_name":"White",
  "occupation" : "1",
  "gender":"m",
  "birthplace" : "Albuquerque",
  "birthday" : "1957-09-07",
  "nationality" : "US",
  "marital_status" : "1",
  "adr_street" : "Negra Arroyo Lane",
  "adr_street_number" : "308",
  "adr_post_code" : "87111",
  "adr_city" : "Albuquerque",
  "adr_country" : "US",
  "tos":true,
  "privacy_policy":true,
  "own_interest":true,
  "us_citizen":true,
  "us_tax_payer":true,
  "newsletter":true,
  "verification_token":"rrfjjEEwooyf1vYg-5rIu-VZVtIegy9DvsNWSq3pVfDIz9jVmUW3UsPnoARvbbhFFMuBHtg"
}
```

Creates a new customer object.


Arguments | Description | Format
--------- | ----------- | -----------
affiliate_uid | Unique identifier of the affiliate for which customer is created | String (e.g. `"1398b666-6666-6666-6666-666666666666"`)
email | The customer's email address - same as the user's email address | String
password | Try to choose a secure password | String
adr_mobile | Tell us your mobile phone number. We won't call you, promise! :) | String in <a href='http://www.msisdn.org/' target='_blank'>MSISDN format</a>
tile | Salutation | Integer (enum), e.g. e.g 1 - `"Mr."` or 2 - `"Mrs."`
first_name | Customer's first name | String
last_name | Customer's last name | String
additional_first_name | If you have an additional name just put it in here | String
occupation | Customer's current occupation | Integer (enum) 1 - employee, 2 - executive, 3 - self-employed, 4 - freelancer, 5 - student, 6 - trainee, 7 - retired, 8 - privatier, 9 - unemployed"
gender | Customer's gender | String (enum) - Valid Values: "m", "f"
birthplace | City of customer's birth | String
birthday | Customer's birthdate e.g. "1973-07-02" | Date
nationality | Customer's nationality - Country code as defined in ISO3166 alpha2. e.g "DE", "GB" | String
marital_status | Customer's marital status | String (enum) - 1: single, 2: married, 3: widowed, 4: divorced, 5: separated, 6: de facto
adr_street | Street address of the customer | String
adr_street_number | House number | String
adr_post_code | Postal code of the customer | String
adr_city | City customer lives in | String
adr_country | Country customer lives in | String
verification_token | Verification token from `/msisdn/verify` call | String e.g. `"rfjjEEwooyf1vYg-5rIu-VZVtIegy9DvsNWSq3pVfDIz9jVmUW3UsPnoARvbbhFFMuBHtg"`
nick | Nickname used in community - can be used as login  | String
maiden_name | Customer's maiden name | String
adr_phone | Customer's phone number | String
adr_mobile | Customer's mobile phone number | String
adr_fax | Customer's fax number | String
adr_businessphone | Customer's business phone number | String
tos | Terms of Service | Boolean
privacy_policy | Privacy Policy | Boolean
own_interest | Identifies if you act in your own interest | Boolean
us_citizen | Please tell us if you hold US citizenship | Boolean
us_tax_payer | And have to pay the taxes in US | Boolean
newsletter | Wanna get Fidor's newsletter? | Boolean

## Accounts

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
Transactions gives you a list of financial transactions that happened on the given account. In most cases not only the obvious transaction related data but also  `transaction_type_details` are provided (see below).

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
Different transaction types contain different details specific for this particular type of the transaction. Some of the transaction types don't have any specific attributes. In this case the `transaction_type_details` object remains empty.

Transaction Type | Description | Transaction Type Details
---- | ---- | ----
fidor_payin | internal payment (in) | internal_transfer_details
fidor_payout | internal payment (out) | internal_transfer_details
emoney_payin | Giropay payment (in) | internal_transfer_details
sepa_payin | SEPA payment (in) | sepa_credit_transfer_details
payout | SEPA payment (out) | sepa_credit_transfer_details
creditcard_* | see below | credit_card_details
sepa_core_direct_debit | SEPA DD payment (out) standard | sepa_credit_transfer_details
sepa_b2b_direct_debit | SEPA DD payment (out) business | sepa_credit_transfer_details
gmt_payout | global money transfer (out) | gmt_details
gmt_refund | GMT refund (in) | gmt_details
gmt_fee | fee for GMT | gmt_details
bonus | Fidor bonus payment (in) | bonus_details
prepaid_mobile_topup | Topup for prepaid phones (out) | mobile_topup_details
credit_interest | interest payment (bank gives) | -
debit_interest | interest payment (bank takes) | -
fee | general fee (out) | -
fee_giropay | fee for Giropay usage (out) | -
unknown | unmapped transaction type | -

Let's take closer look at the transaction types Fidor supports currently.


###Internal Transfer
Internal transfers are closed loop transaction (payments) from one Fidor bank account to another.
Details of the `internal_transfer` object contain extensive information about the transaction's initiator.

####fidor_payin

> fidor_payin

```json
{
  "internal_transfer_id": "666",
  "remote_account_id": "66666666",
  "remote_bic": "FDDODEMMXXX",
  "remote_iban": "DE73700222000066666666",
  "remote_name": "Walther White",
  "remote_nick": "Heisenberg",
  "remote_subject": "Thanks for nothing"
}
```

In case of fidor_payin you will receive more information about the initiator of the payment.

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
{
  "internal_transfer_id": "666",
  "remote_account_id": "66666666",
  "remote_bic": "",
  "remote_iban": "",
  "remote_name": "",
  "remote_nick": "Heisenberg",
  "remote_subject": "Thanks for nothing"
}
```

`fidor_payout` has the same structure but fewer details because of legal reasons.

Parameter | Description | Format
--------- | ----------- | -----------
internal_transfer_id  | Unique account identifier of the transfer transaction belongs to. Refunded transactions have no id | String
remote_account_id     | Unique account identifier of the payment's sender | String
remote_bic            | empty | String
remote_iban           | empty | String
remote_name           | empty | String
remote_nick           | Community nickname of the transaction's sender | String
remote_subject        | Subject of the transaction | String

###SEPA Credit Transfer
Details of the `sepa_credit_transfers` object contain extensive information about the transaction's initiator or receiver. We differentiate between the incoming `sepa_payin` and outgoing `payout` SEPA transactions.

####sepa_payin or payout

> sepa_payin, payout

```json
{
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

###SEPA Direct Debit (Lastschrift)
SDDs are payments that are automatically drawn from an account. There are two kinds: SEPA-Basislastschriften (SEPA Direct Debit CORE/COR1) and SEPA-Firmenlastschrift (SEPA Direct Debit B2B) which result in different transaction types: `sepa_core_direct_debit` and `sepa_b2b_direct_debit`. Both use the `sepa_credit_transfer` details object as described under *SEPA Credit Transfer* above.


###Credit Card
Credit card usage is a complex topic with many different transaction types ranging from preauthentication to fees.
All types share use the same  `credit_card` object.

Transaction Type | Description
--- | ---
creditcard_preauth | Pre-authorize and block amount (*depricated, see* `preauth`)
creditcard_release | Release pre-authorization and unblock amount  (*depricated, see* `preauth`)
creditcard_payout | Credit card payment (out)
creditcard_payin | Credit card payment (in)
creditcard_annual_fee | Annual card fee
creditcard_foreign_exchange_fee | Fee for using the CC in a foreign country
creditcard_order_fee | Fee for card ordering
creditcard_order_cancellation | If card order is canceled, fee gets refunded
creditcard_order_withdrawal_fee | Fee for cancelling card order
creditcard_atm_fee | Fee for using the card at the ATM
creditcard_notification_fee | Fee for transaction notification (e-mail, SMS)


> credit_card details

```json
{
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

###Global Money Transfer (GMT)
GMT payments are payments to countries outside the SEPA area. Details of the `gmt_transfer` object contain information about the transaction's target.
####gmt_payout, gmt_refund, gmt_fee
> gmt_payout, gmt_refund, gmt_fee

```json
{
  "destination_country": "AU",
  "destination_currency": "AUD",
  "amount_in_destination_currency": 12500,
  "exchange_rate": 1.4591,
  "fee_transaction_id": 2452334
}
```

Parameter | Description | Format
--------- | ----------- | -----------
destination_country | Destination country, 2 letter (ISO3166 alpha2) | String
destination_currency | Destination currency, 3 letter upcase (ISO 4217)| String
amount_in_destination_currency | Amount transferred in the destination currency | Integer
exchange_rate | Exchange rate (EUR/dest_curr) valid at the time of transfer | Decimal
fee_transaction_id | ID of  fee transaction of this GMT transaction | Integer

###Bonus
Some activities in the community lead to bonus payments. The details are stored in the `bonus_details` object.

####bonus

> bonus

```json
{
  "affiliate_id" : "16",
  "affiliate_name" : "ficoba_community",
  "affiliate_transaction_type_id" : "46",
  "affiliate_transaction_type_name" : "Geldfrage beantworten",
  "affiliate_transaction_type_category" : "Community"
}
```

Parameter | Description | Format
--------- | ----------- | -----------
affiliate_id | ID of the bonus affiliate | String
affiliate_name | Name of the bonus affiliate | String
affiliate_transaction_type_id | Identifier of the transaction type | String
affiliate_transaction_type_name | name of the transaction type | String
affiliate_transaction_type_category | category, e.g. Community, Banking, Bonusprogramm | String

###Mobile Topup
If you use the topup app in the account, transactions will be marked as `prepaid_mobile_topup`. The details are stored in the `mobile_topup_details` object.

####prepaid_mobile_topup

> prepaid_mobile_topup

```json
{
  "provider" : "vodafone",
  "phone_number" : "1234567890123",
  "topup_subject" : "Nada DE Vodafone"
}
```

Parameter | Description | Format
--------- | ----------- | -----------
provider | name of the mobile network operator | String
phone_number | Mobile phone number user for topup | String
topup_subject | Subject of the mobile topup | String

##Transactions Filter
There are many ways to filter the output of `transactions`.
> GET https://api.fidor.de/transactions?filter[transaction_types]=Value&filter[booking_date_from]=Value

Using the filter returns only entries with ... (see description)

Name | Type | Description
--------- | ----------- | -----------
filter[id_from] | string (integer) | ... ids >= the given id
filter[id_to] | string (integer) | ... ids <= the given id
filter[booking_date_from] | string (date-time) | ... booking_date >= the given date
filter[booking_date_to] | string (date-time)  | ... booking_date <= the given date
filter[transaction_types]  | string (enum), see transaction_types | ... the given type

## Preauths
Prauths are blocked amounts that have been reserved for future payments. The money has not yet have been deducted from the account but is not available for other spending. This is usually used with credit cards but there are a lot of other use cases as well.
The sum of all current preauths can be seen with `accounts.preauth_amount`.

> GET https://api.fidor.de/preauths/:id

```json
{
  "id": "9123",
  "account_id": "1238343434",
  "amount": 2435,
  "currency": "EUR",
  "expires_at": "2015-02-03T13:02:45+01:00",
  "preauth_type": "creditcard_preauth",
  "preauth_type_details": {
    "cc_merchant_name": "Starbucks",
    "cc_merchant_category": "O",
    "cc_type": "IO",
    "cc_category": "C",
    "cc_sequence": "25",
    "pos_code": "POS",
    "financial_network_code": "CAN"
  },
  "created_at": "2015-02-03T06:23:43Z",
  "updated_at": "2015-02-03T06:23:43Z"
}
```

Parameter            | Description                                                                                                                                               | Format
---------            | -----------                                                                                                                                               | -----------
id                   | Unique preauth identifier                                                                                                                                 | String
account_id           | Fidor account the preauth belongs to                                                                                                                      | String
preauth_type         | Type of the preauth                                                                                                                                       | String (enum)
amount               | The blocked amount in account currency, in minor units, e.g. 1 EUR is represented as 100. Will be set to 0 once preauth has been cleared. Can be negative e.g. if something was withdrawn from an account. | Integer
expires_at           | Expiration date (if not cleared before). Default expiring time is 30 days. ISO 8601 Date-Time                                                                                                          | Datetime
created_at           | Creation date-time, never changes. ISO 8601 Date-Time e.g. "2014-10-10T17:41:58+02:00"                                                                    | Datetime
updated_at           | Last update date-time. ISO 8601 Date-Time e.g. "2015-02-04T04:08:54+01:00"                                                                                | Datetime
currency             | Currency of account or amount. ISO 4217 alpha-3 - 3 letter upcase e.g EUR                                                                                 | String (enum)
preauth_type_details | Details specific to this preauth type are collected here                                                                                                  |


## Preauth Type Details
Different preauth types provide different preauth type details.

Preauth Type              | Description           | Preauth Type Details
----                      | ----                  | ----
creditcard_preauth        |                       | credit_card_details
internal_transfer_preauth |                       | internal_transfer_details
capital_bond_preauth      |                       | capital_bond_details
currency_order_preauth    |                       | currency_order_details
gmt_preauth               |                       | gmt_details
ripple_preauth            |                       | ripple_details
unknown                   | unmapped preauth type | -

Let’s take closer look at the preauth types Fidor supports currently.

### Credit Card
Details of the `creditcard_preauth` object contain information about the merchant and more.

```json
{
  "cc_category": "R",
  "cc_merchant_category": "5411",
  "cc_merchant_name": "Metro Cash & Carry",
  "cc_sequence": 6666666,
  "cc_type": "00",
  "pos_code": "COD",
  "financial_network_code": "IXC23"
}
```

Parameter | Description | Format
--------- | ----------- | -----------
cc_category          | CreditCard category | String
cc_merchant_category | Category given by the merchant | String
cc_merchant_name     | CreditCard merchant initiating the transaction | String
cc_sequence          | Sequence links all the credit_card related transactions together | String
cc_type              | CreditCard type | String
pos_code  | Code for point of service where card was used | String
financial_network_code | empty | String

### Internal Transfer
If internal money transfers cannot be completed (e.g. because the receiver does not have a Fidor account yet), the amount is blocked until the money is collected or the preauth expires. Details of the `internal_transfer_preauth` object contain information about the receiver of the payment.

```json
{
  "internal_transfer_id": "666",
  "remote_account_id": "66666666",
  "remote_bic": "",
  "remote_iban": "",
  "remote_name": "",
  "remote_nick": "Heisenberg",
  "remote_subject": "Thanks for nothing"
}
```

Parameter            | Description                                                                                        | Format
---------            | -----------                                                                                        | -----------
internal_transfer_id | Unique account identifier of the transfer transaction belongs to. Refunded transactions have no id | String
remote_account_id    | Unique account identifier of the payment's sender                                                  | String
remote_bic           | empty                                                                                              | String
remote_iban          | empty                                                                                              | String
remote_name          | empty                                                                                              | String
remote_nick          | Community nickname of the transaction's sender                                                     | String
remote_subject       | Subject of the transaction                                                                         | String

### Capital Bond
If you order a Fidor capital bond the amount gets blocked until the bond gets approved by Fidor. Details of the `capital_bond_preauth` object contain information about the bought capital bond.


### Currency Order
If you buy foreign currencies with the in-account app in the web interface of your Fidor Smart Account, the amount of the purchase will be blocked until the payment process is completed. There is no API for currency order yet. Details of the `currency_order_preauth` object contain information about the currency order.

### Global Money Transfer (GMT)
If you use Global Money Transfer (Auslandsüberweisung) the amount will be blocked until the transfer process is completed. Details of the `gmt_preauth` object contain information about the transaction's target.

```json
{
  "destination_country": "AU",
  "destination_currency": "AUD",
  "amount_in_destination_currency": 12500,
  "exchange_rate": 1.4591,
  "fee_transaction_id": 2452334
}
```

Parameter                      | Description                                                 | Format
---------                      | -----------                                                 | -----------
destination_country            | Destination country, 2 letter (ISO3166 alpha2)              | String
destination_currency           | Destination currency, 3 letter upcase (ISO 4217)            | String
amount_in_destination_currency | Amount transferred in the destination currency              | Integer
exchange_rate                  | Exchange rate (EUR/dest_curr) valid at the time of transfer | Decimal
fee_transaction_id             | ID of  fee transaction of this GMT transaction              | Integer

### Ripple details
If you send money via Ripple the amount will be blocked until the transfer has been completed. Details of the `ripple_preauth` object contain information about the transaction's target.

##Preauth Filter
There are many ways to filter the output of `preauth`.
> GET https://api.fidor.de/preauth?filter[transaction_types]=Value&filter[booking_date_from]=Value

Using the filter returns only entries with ... (see description)

Name | Type | Description
--------- | ----------- | -----------
filter[id_from] | string (integer) | ... ids >= the given id
filter[id_to] | string (integer) | ... ids <= the given id
filter[booking_date_from] | string (date-time) | ... booking_date >= the given date
filter[booking_date_to] | string (date-time)  | ... booking_date <= the given date
filter[transaction_types]  | string (enum), see transaction_types | ... the given type
