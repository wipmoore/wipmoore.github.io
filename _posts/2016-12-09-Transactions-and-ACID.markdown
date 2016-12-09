---
layout: post
title:  "Database Transactions and ACID"
date:   2016-12-09 10:00:00 +0100
categories: jekyll update
---

## ACID 

* **Atomic**, a transactions is all or nothing, if one part of a transaction fails then the entire transaction fails.
* **Consistent**, the database will go from one valid state to another.
* **Isolation**, concurrent execution of transactions result in a system state that would be obtained if transactions were executed serially.
* **Durability**, Once a transaction has been commited it will remain so. 

## Transaction Isolation levels

These are levels that define how isolated the Isolation property of a transaction is.  **Note** they are transactions specific, different transactions can have different isolation levels.


### Serializable

* Statements can not read data that has been modified but not yet committed by other transactions.
* No other transaction can modify data the has been read by the current transaction unitl the current transaction completes.
* Other transactions cannot insert new rows with key values that would fall in the range of keys read by any statement in the current transaction.


### Repeatable read

* Statement can not read data that has been modified but not yet committed by other transactions ( No dirty reads ).
* No other transaction can modify data the has been read by the current transaction.

**Note** You can get phantom rows as inserts from other transactions are allowed.   


### Read committed

* Statements can not read data thtat has been modified but not commited by other transactions  ( No dirty reads).

**Note** Data can be changed by other transactions between individual statements within the current transaction resulting in nonrepeatable reads ( Phantom Data ).


### Read uncommitted

* Statments can read rows that have been modified by other transactions but not yet committed.

# References

* https://en.wikipedia.org/wiki/ACID
* https://msdn.microsoft.com/en-us/library/ms173763.aspx
