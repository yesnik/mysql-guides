# Detect locks

## Show InnoDb status
```sql
SHOW ENGINE innodb STATUS;
```

Look at the transactions section:

```
------------
TRANSACTIONS
------------
Trx id counter 2F5C54CCE
Purge done for trx's n:o < 2F5C34FDF undo n:o < 0
History list length 12244
LIST OF TRANSACTIONS FOR EACH SESSION:
---TRANSACTION 2F5C54CCA, not started
MySQL thread id 293967, OS thread handle 0x7ff1f89c9700, query id 23377278 localhost sales_web
---TRANSACTION 2F5C54CCD, not started
---TRANSACTION 2F5C54C9F, ACTIVE 0 sec fetching rows
mysql tables in use 11, locked 11
1037 lock struct(s), heap size 145848, 317551 row lock(s)
MySQL thread id 292739, OS thread handle 0x7ff1f8ea2700, query id 23377229 localhost root Sending data
UPDATE submission_cc_dominanta SET smtp_addr='EvgeniyKozlov-717@list.ru' WHERE id='49027580'
TABLE LOCK table `sales`.`claims_cc` trx id 2F5C54C9F lock mode IS
---TRANSACTION 2F5C54C9F, ACTIVE 0 sec fetching rows
mysql tables in use 11, locked 11
1037 lock struct(s), heap size 145848, 317551 row lock(s)
MySQL thread id 292739, OS thread handle 0x7ff1f8ea2700, query id 23377229 localhost root Sending data
UPDATE submission_cc_corp SET smtp_addr='hello@gmail.com' WHERE id='49027580'
TABLE LOCK table `sales`.`claims_cc` trx id 2F5C54C9F lock mode IS
RECORD LOCKS space id 0 page no 16782473 n bits 1272 index `created` of table `sales`.`claims_cc` trx id 2F5C54C9F lock mode S
RECORD LOCKS space id 0 page no 12271147 n bits 192 index `PRIMARY` of table `sales`.`claims_cc` trx id 2F5C54C9F lock mode S locks rec but not gap
RECORD LOCKS space id 0 page no 12271148 n bits 176 index `PRIMARY` of table `sales`.`claims_cc` trx id 2F5C54C9F lock mode S locks rec but not gap
RECORD LOCKS space id 0 page no 6632572 n bits 288 index `PRIMARY` of table `sales`.`claims_cc` trx id 2F5C54C9F lock mode S locks rec but not gap
RECORD LOCKS space id 0 page no 6632526 n bits 184 index `PRIMARY` of table `sales`.`claims_cc` trx id 2F5C54C9F lock mode S locks rec but not gap
RECORD LOCKS space id 0 page no 6632513 n bits 192 index `PRIMARY` of table `sales`.`claims_cc` trx id 2F5C54C9F lock mode S locks rec but not gap
RECORD LOCKS space id 0 page no 12271151 n bits 272 index `PRIMARY` of table `sales`.`claims_cc` trx id 2F5C54C9F lock mode S locks rec but not gap
RECORD LOCKS space id 0 page no 16782474 n bits 1272 index `created` of table `sales`.`claims_cc` trx id 2F5C54C9F lock mode S
RECORD LOCKS space id 0 page no 12271152 n bits 280 index `PRIMARY` of table `sales`.`claims_cc` trx id 2F5C54C9F lock mode S locks rec but not gap
TOO MANY LOCKS PRINTED FOR THIS TRX: SUPPRESSING FURTHER PRINTS
```

## Info about InnoDb transactions

This query will show current queries, number of locked tables, rows, isolation level etc.

```sql
SELECT * from INFORMATION_SCHEMA.INNODB_TRX
```

## Show open tables

This query will show names of currently used tables:

```sql
show open tables where In_Use > 0 ;
```
