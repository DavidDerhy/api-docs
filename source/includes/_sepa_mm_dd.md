#Sepa Mandates and Direct Debits

In order to create a SEPA Direct Debit you have to create a SEPA Mandate first. Not every Fidor customer is eligible to create SEPA Mandates. If you are interested in SEPA Direct Debits and do not have a required account for creating SEPA Mandates you will need to contact Fidor first.

To be able to create mandates you have to acquire a Unique Creditor Identifier.

The Creditor Identifier for Germany is precisely 18 characters in length and is structured as follows

- The first two characters represent the ISO country code for Germany (DE) as the country issuing the Creditor Identifier.
- The third and fourth characters are the check digits which are calculated in compliance with the IBAN check digits (ISO 13616) without taking characters five to seven (Creditor Business Code) into account.
- The fifth, sixth and seventh characters in the sequence signify the Creditor Business Code for which the direct debit creditor may select alphanumeric characters to denote individual business areas or branches specific to his requirements. The use of blanks, special characters and umlauts is not permitted. As a rule, the three characters entered here are the letters “ZZZ”.
- The remaining characters 8 to 18 signify the national identifier for the direct debit creditor and are numbered in consecutive ascending order. Until further notice, the eighth character in the Creditor Identifier will always be "0".

You can find more information about how to acquire a UCI (Unique Creditor Identifier) by following this link:
[Creditor Identifier]( https://www.bundesbank.de/Redaktion/EN/Standardartikel/Tasks/Payment_systems/sepa_creditor_identifier.html)

## HTTP Requests

`GET http://api.fidor.de/creditor_identities` <sub>index</sub>

`GET http://api.fidor.de/sepa_mandates` <sub>index</sub>

`GET http://api.fidor.de/sepa_mandates/{id}` <sub>self</sub>

`POST http://api.fidor.de/sepa_mandates` <sub>create</sub>

`PUT http://api.fidor.de/sepa_mandates/{id}` <sub>update</sub>

`GET https://api.fidor.de/sepa_direct_debits` <sub>index</sub>

`GET https://api.fidor.de/sepa_direct_debits/{id}` <sub>self</sub>

`POST https://api.fidor.de/sepa_direct_debits` <sub>create</sub>

##Creditor Identifiers

> GET http://api.fidor.de/creditor_identities

```
{
  "data": [
    {
      "id": "GB56ZZZDFFGFFO00000020150006",
      "account_id": "12345678",
      "created_at": "2015-04-07T13:33:59Z",
      "updated_at": "2015-04-07T15:54:48Z"
    },
    {
      "id": "DE37ZZZ08000000001",
      "account_id": "12345678",
      "created_at": "2015-04-07T13:46:08Z",
      "updated_at": "2015-04-08T08:45:33Z"
    }
  ],
  "collection": {
    "current_page": 1,
    "per_page": 10,
    "total_entries": 3,
    "total_pages": 1
  }
}
```

Once you acquired one or multiple UCIs (Unique Creditor Identifiers) you have to contact Fidor.
We will store your UCIs related credentials in our system.
To get the information about Creditor Identifiers available for your use, you have to call the `/creditor_identities` endpoint.

You will have to use one of the available `creditor_identities` to create `sepa_mandates`.

Parameter | Description | Format
--------- | ----------- | -----------
id         | Unique Creditor Identifier | String
account_id | Account identifier of the sender (`/accounts/id`)| String
created_at | Creation date-time, never changes | String (date-time) ISO 8601 Date-Time
updated_at | Last update date-time | String (date-time) ISO 8601 Date-Time

##Creating SEPA Mandate

Once the SEPA mandate is stored successfully, you have the possibility to change the mandate by issuing a `PUT` request to `/sepa_mandates/:id`.

> POST http://api.fidor.de/sepa_mandates

```
{       
  "creditor_identity_id" : "DE37ZZZ08000000001",
  "external_uid" : "666",
  "mandate_reference": "666",
  "sequence": "RCUR",
  "cor1_option": "1",
  "remote_title": "Mr.",
  "remote_name": "Walter White",
  "remote_email": "walter@heisenberg.com",
  "remote_address_line1": "308 Negra Arroyo Lane",
  "remote_address_line2": "Albuquerque, New Mexico, 87104",
  "remote_country": "US",
  "remote_iban": "DE08100100100666666666",
  "remote_bic": "FDDODEMMXXX",
  "signature_date": "2015-04-07",
  "valid_from_date": "2015-11-18"
}
```

> GET http://api.fidor.de/sepa_mandates/:id

```
{
  "id" : "66666",
  "customer_id" : "66666666",
  "creditor_identity_id" : "DE37ZZZ08000000001",
  "external_uid" : "666",
  "mandate_reference": "666",
  "sequence": "RCUR",
  "cor1_option": "1",
  "remote_title": "Mr.",
  "remote_name": "Walter White",
  "remote_email": "walter@heisenberg.com",
  "remote_address_line1": "308 Negra Arroyo Lane",
  "remote_address_line2": "Albuquerque, New Mexico, 87104",
  "remote_country": "US",
  "remote_iban": "DE08100100100666666666",
  "remote_bic": "FDDODEMMXXX",
  "remote_ultimate_name": null,
  "remote_signature_location": null,
  "signature_date": "2015-04-07",
  "valid_from_date": "2015-11-18",
  "created_at": "2015-11-25T12:43:55Z",
  "updated_at": "2015-11-25T12:43:55Z"
}
```

> PUT http://api.fidor.de/sepa_mandates/:id

```
{
  "remote_title": "Mr.",
  "remote_name": "Walter Black",
  "remote_email": "no@drugs.com",
  "remote_address_line1": "545 Fifth Avenue",
  "remote_address_line2": "New York, New York, 10021",
  "remote_country": "US"
}
```

Parameter | Description | Format
--------- | ----------- | -----------
creditor_identity_id | Unique Creditor Identifier | String
external_uid | Unique ID of the creator of the transfer. In case a uid is reused for a transfer, it is not executed, this mechanism can be used to prevent double bookings in case of network failure or similar event where transfer status is unknown | String
mandate_reference | Unique Reference of the Mandate | String
sequence | Recurring or One-Time | String ("RCUR" or "OOFF")
cor1_option | Use COR-1 Option | String ("0" or "1")
remote_title | Title / Salutation of Debtor | String ("Mr., "Mrs.")
remote_name | Full name of the debtor | String
remote_email | Email Address of the debtor | String
remote_address_line1 | Street and street number of the debtor | String
remote_address_line2 | ZIP and City of the debtor | String
remote_country | Country of the | String (Country Code)
remote_iban | IBAN of the transaction's debtor | String
remote_bic | BIC of the transaction's debtor | String
signature_date | Date of mandate signature | String (Date)
valid_from_date | Date, mandate is valid from | String (Date)

##Creating SEPA Direct Debit

> POST https://api.fidor.de/sepa_direct_debits

```
{
  	"external_uid" : "666",
    "mandate_id": "666",
  	"account_id" : "66666666",
  	"eref" : "6",
  	"amount" : "10000",
  	"subject" : "We're taking your money back, Walter!",
  	"collection_date" : "2015-04-27"
}
```

> GET https://api.fidor.de/sepa_direct_debits

```
{
  "data": [
    {
      "account_id": "66666666",
      "amount": 6666,
      "collection_date": "2016-06-09T00:00:00+00:00",
      "creditor_identity_id": "DE37ZZZ08000000001",
      "currency": "EUR",
      "eref": "WW-heisenberg-1234",
      "external_uid": "d9de75bf-d381-49d7-92a5-666666666666",
      "mandate_id": "66666",
      "state": "success",
      "subject": "Heisenbergio: 123456789",
      "user_id": "666666",
      "id": "66666",
      "created_at": "2016-06-08T09:41:15Z",
      "updated_at": "2016-06-08T09:41:19Z"
    },
    {
      "account_id": "66666666",
      "amount": 66666666666,
      "collection_date": "2016-06-09T00:00:00+00:00",
      "creditor_identity_id": "DE37ZZZ08000000001",
      "currency": "EUR",
      "eref": "WW-heisenberg-12345",
      "external_uid": "d9de75bf-d381-49d7-92a5-666666666667",
      "mandate_id": "66666",
      "state": "success",
      "subject": "Heisenbergio: 123456790",
      "user_id": "666666",
      "id": "66667",
      "created_at": "2016-06-08T08:11:48Z",
      "updated_at": "2016-06-08T08:11:51Z"
    }
  ]
}
```

Once you created a `sepa_mandate` you can go on and create a corresponding `sepa_direct_debit`. In order to do so you have to make a `POST` request to `/sepa_direct_debits` endpoint with following request body.

Parameter | Description | Format
--------- | ----------- | -----------
id | Unique Direct Debit identifier | String
external_uid | Unique ID of the creator of the transfer. In case a uid is reused for a transfer, it is not executed, this mechanism can be used to prevent double bookings in case of network failure or similar event where transfer status is unknown | String
mandate_id | Unique Reference of the Mandate | String
account_id | Account identifier of the sender | String
amount | Amount of money you would like to debit from the debtor, in minor units, e.g. 1EUR is represented as 100. | Integer
currency |Currency of Account or Amount. ISO 4217 alpha-3 - 3 letter upcase e.g EUR | String (enum)
subject | Subject of the direct debit | String
collection_date | Date of the money collection | String (Date)
creditor_identity_id | Unique Creditor Identifier | String
currency | Currency of amount. ISO 4217 alpha-3 - 3 letter upcase e.g EUR| String
eref | And end-to-end reference of the created direct debit | String
state | A status indicator. Following statuses are supported: `received`, `processing`, `success`, `failure` | String (enum)
user_id | User ID of the Direct Debit creator | String
created_at | Creation date-time, never changes | String (date-time) ISO 8601 Date-Time
updated_at | Last update date-time | String (date-time) ISO 8601 Date-Time

##Batch Direct Debit

*PREVIEW:* A direct debit batch contains multiple direct debits which are processed asynchronously.

`GET https://api.fidor.de/batch_direct_debits/{id}` <sub>self</sub>

`GET https://api.fidor.de/batch_direct_debits` <sub>instances</sub>

`POST https://api.fidor.de/batch_direct_debits` <sub>create</sub>
