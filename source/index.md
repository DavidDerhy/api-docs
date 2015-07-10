---
title: Fidor API Reference

toc_footers:
  - <a href='https://apm.fidor.de'>Application Manager</a>
  - <a href='https://developer.fidor.de/community/'>Developer Community</a>
  - <a href='https://developer.fidor.de/api-browser/'>API Browser</a>
  - <a href="https://www.fidor.de/impressum">Imprint</a>

includes:
  - getting_started
  - develop_debug
  - approval_operation
  - basic_data
  - money_transfer
  - trust_account
  - errors

search: true
---

# Introduction
```
  /ffffff   /ffffff  /ff
 /ff__  ff /ff__  ff| ff
| ff  \ ff| ff  \ ff| ff
| ffffffff| ffffff  | ff
| ff__  ff| ff      | ff
| ff  | ff| ff      | ff
| ff  | ff| ff      | ff
|__/  |__/|__/      |__/
```
Welcome to the Fidor Banking API! Our API allows you to easily access your Fidor bank account, get information about your transaction history and submit various types of payments. In addition you may provide services for Fidor customers and access their accounts if certain conditions are met.

For general introduction to our developer program please visit our [developer community](https://developer.fidor.de/).

Our API is RESTful, we use JSON format and OAuth2.0 authorization.

## Endpoints
Here's a quick overview of our API endpoints:

Method | Endpoint | Usage | Returns
--------- | ----------- | --------- | -----------
GET | [`/users/current`](#users) | Get user's data | users
GET | [`/customers`](#customers) | Get customer data | customers
GET | [`/accounts`](#accounts) | Get customer's bank accounts | accounts
POST | [`/internal_transfers`](#internal-transfer) | Send money to another fidor user |
GET | [`/internal_transfers`](#internal-transfer) | Get all fidor-to-fidor transfers for the current user | internal transfer
POST | [`/sepa_credit_transfers`](#sepa-credit-transfer) | Send money to another bank account through SEPA |
GET | [`/sepa_credit_transfers`](#sepa-credit-transfer) | Get all sepa transfers for the current user | sepa credit  transfer
POST | [`/batch_transfers`](#batch-transfer) | Send money in batch either with internal or sepa credit transfer or both |
GET | [`/batch_transfers`](#batch-transfer) | Get all batch transfers for the current user | batch transfer
GET | [`/transactions`](#transactions) | Get all transactions for the current user | transaction


## Systems
To avoid confusion here's a list of the Fidor systems and URLs you will be dealing with with:

URL | System
----- | -----
www.fidor.de | Banking homepage, you can register here
apm.fidor.de | Application management environment for developers
aps.fidor.de | Sandbox endpoint (test environment)
api.fidor.de | Banking production  endpoint (live environment)
docs.fidor.de | Developer documentation (*You are here*)
developer.fidor.de | Developer community landing page

There are two environments with their respective security services:

Environment | Login (Authentication) | OAuth (Authorization) | API
----- | ----- | ----- | -----
Testing/Simulation | aps.fidor.de (with sandbox accounts/logins)| aps.fidor.de | aps.fidor.de
Production | banking.fidor.de (with real accounts/logins) | apm.fidor.de | api.fidor.de
