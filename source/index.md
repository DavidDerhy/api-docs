---
title: Fidor API Reference

toc_footers:
  - <a href='https://apm.fidor.de'>Get API Credentials</a>

includes:
  - introduction
  - basic_data
  - money_transfers
  - errors

search: true
---

# Fidor Banking API
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
Welcome to the Fidor Banking API! Our API allows you to easily access your Fidor bank account, get information about your transaction history and submit various types of payments. In addition you may provide services your for Fidor customers and access their accounts if certain conditions are met.

For general introduction to our developer program please visit our [developer community](https://developer.fidor.de/)

Method | Endpoint | Usage | Returns
--------- | ----------- | --------- | -----------
GET | [`/users/current`](#user) | Get user's data | user
GET | [`/customers`](#customer) | Get customer data | customer
GET | [`/accounts`](#account) | Get customer's bank accounts | account
POST | [`/internal_transfers`](#internal-transfer) | Send money to another fidor user |
GET | [`/internal_transfers`](#internal-transfer) | Get all fidor-to-fidor transfers for the current user | internal transfer
POST | [`/sepa_credit_transfers`](#sepa-credit-transfers) | Send money to another bank account through SEPA |
GET | [`/sepa_credit_transfers`](#sepa-credit-transfers) | Get all sepa transfers for the current user | sepa credit  transfer
POST | [`/batch_transfers`](#batch-transfers) | Send money in batch either with internal or sepa credit transfer or both |
GET | [`/batch_transfers`](#batch_transfers) | Get all batch transfers for the current user | batch transfer
<!-- coming soon - more or less
POST | [`/sepa_mandates`](#sepa_mandates) | Create sepa mandate |
GET | [`/sepa_mandates`](#sepa_mandates) | Get all previously created sepa mandates for the current user | sepa mandate
POST | [`/sepa_direct_debits`](#sepa_direct_debits) | Create sepa direct debit for a sepa mandate |
GET | [`/sepa_direct_debits`](#sepa_direct_debits) | Get all previously created sepa direct debits for the current user | sepa direct debit
POST | [`/batch_direct_debits`](#batch_direct_debits) | Create batch of sepa direct debits |
GET | [`/batch_direct_debits`](#batch_direct_debits) | Get all previously created batches of sepa direct debits for the current user | batch direct debit 
-->


