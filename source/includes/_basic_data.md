#Basic Data

##API Rate Limits
> GET https://api.fidor.de/rate_limit

```json 
{
    "limit": 60,
    "remaining": 55,
    "reset": 155
}
```

> In case you exceeded the rate limit:

```json
{
    "code": 429,
    "message": "Rate limit exceeded. Please retry later."
}
```

We throttle our APIs by default to ensure maximum performance for all developers.

Rate limiting in version 1 of the API is primarily considered on a per-user basis — or more accurately described, per access token in your control. Rate limits are determined globally for the entire application.

Rate limits in version 1 of the API are divided into 15 minute intervals. In the version 1 of the API rate limit is 60 calls every 15 minutes, but we may adjust that over time.

###HTTP Headers
For you to make a better prediction about the API rate limit for your application or user, please consider to use the `X-RateLimit-*` headers.

We provide three context based headers for this purpose: 

HTTP Header | Description
--------- | -----------
X-RateLimit-Limit   | The rate limit ceiling for all requests / 15-min window
X-RateLimit-Remaining | The number of requests left for the the 15-min window
X-RateLimit-Reset | The remaining window before the rate limit resets in e.g. UTC epoch seconds

When an application exceeds the rate limit, the Fidor API will return an HTTP 429 “Too Many Requests” response code (HTTP Status Code Documentation).

For your convenience we provide you an endpoint for requesting the current state of your rate limit `GET https://api.fidor.de/rate_limit`

##Pagination
> https://api.fidor.de/transactions?page=2&per_page=100

```
X-LIMIT → 100
X-OFFSET → 100
X-TOTAL → 300
```

We use the header based pagination for navigating through long lists of found entries.

HTTP Header | Description
--------- | -----------
X-LIMIT   | The per page limit of the entries shown. Default value: 50
X-OFFSET | The pagination offset
X-TOTAL | The total number of the entries

To navigate through the pages please use `page=` and `per_page=` URL parameters as stated in an example on the right pane.


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
