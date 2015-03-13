---
title: Fidor API Reference

toc_footers:
  - <a href='https://apm.fidor.de'>Get API Credentials</a>
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - getting_started
  - develop_debug
  - approval_operation
  - basic_data
  - money_transfer
  - errors

search: true
---

# Introduction
```
  _          _ _             
 | |__   ___| | | ___        
 | '_ \ / _ \ | |/ _ \       
 | | | |  __/ | | (_) |      
 |_| |_|\___|_|_|\___/   
 
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
POST | [`/sepa_credit_transfers`](#sepa-credit-transfer) | Send money to another bank account through SEPA |
GET | [`/sepa_credit_transfers`](#sepa-credit-transfer) | Get all sepa transfers for the current user | sepa credit  transfer
POST | [`/batch_transfers`](#batch-transfer) | Send money in batch either with internal or sepa credit transfer or both |
GET | [`/batch_transfers`](#batch-transfer) | Get all batch transfers for the current user | batch transfer
POST | [`/sepa_mandates`](#sepa_mandate) | Create sepa mandate |
GET | [`/sepa_mandates`](#sepa_mandate) | Get all previously created sepa mandates for the current user | sepa mandate
POST | [`/sepa_direct_debits`](#sepa_direct_debit) | Create sepa direct debit for a sepa mandate |
GET | [`/sepa_direct_debits`](#sepa_direct_debit) | Get all previously created sepa direct debits for the current user | sepa direct debit
POST | [`/batch_direct_debits`](#batch_direct_debit) | Create batch of sepa direct debits |
GET | [`/batch_direct_debits`](#batch_direct_debit) | Get all previously created batches of sepa direct debits for the current user | batch direct debit 
GET | [`/overdrafts`](#overdraft) | Get the history of overdrafts taken for the current user | overdraft
GET | [`/overdrafts/current`](#overdraft) | Get the current overdraft line for the current user | overdraft
POST | [`/overdrafts`](#overdraft) | Request an overdraft for a certain user account | 
PUT | [`/overdrafts/:id`](#overdraft) | Deactivate the current overdraft | 


