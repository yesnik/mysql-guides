# Detect locks

## Show InnoDb status
```sql
SHOW ENGINE innodb STATUS;
```

Look at `LATEST DETECTED DEADLOCK` section:

```
------------------------
LATEST DETECTED DEADLOCK
------------------------
190311  4:55:53
*** (1) TRANSACTION:
TRANSACTION 427C047E9, ACTIVE 0 sec starting index read
mysql tables in use 1, locked 1
LOCK WAIT 2 lock struct(s), heap size 1248, 1 row lock(s)
MySQL thread id 13858, OS thread handle 0x7f5221746700, query id 937243 192.168.100.25 salesc7 updating
DELETE FROM `claims_settings` WHERE claim_id IN ('40309331', '40309332', '40309333', '40309334', '40309335', '40309336', '40309337', '40309338', '40309339', '40309340', '40309341', '40309342', '40309343', '40309344', '40309345', '40309346', '40309347', '40309348', '40309349', '40309350')
*** (1) WAITING FOR THIS LOCK TO BE GRANTED:
RECORD LOCKS space id 0 page no 14977208 n bits 1232 index `claim_id` of table `sales`.`claims_settings` trx id 427C047E9 lock_mode X locks rec but not gap waiting
*** (2) TRANSACTION:
TRANSACTION 427C047EA, ACTIVE 0 sec starting index read
mysql tables in use 1, locked 1
3 lock struct(s), heap size 1248, 2 row lock(s)
MySQL thread id 13703, OS thread handle 0x7f5222a61700, query id 937244 192.168.100.25 salesc7 updating
DELETE FROM `claims_settings` WHERE claim_id IN ('40309331', '40309332', '40309333', '40309334', '40309335', '40309336', '40309337', '40309338', '40309339', '40309340', '40309341', '40309342', '40309343', '40309344', '40309345', '40309346', '40309347', '40309348', '40309349', '40309350')
*** (2) HOLDS THE LOCK(S):
RECORD LOCKS space id 0 page no 14977208 n bits 1232 index `claim_id` of table `sales`.`claims_settings` trx id 427C047EA lock_mode X locks rec but not gap
*** (2) WAITING FOR THIS LOCK TO BE GRANTED:
RECORD LOCKS space id 0 page no 14977208 n bits 1232 index `claim_id` of table `sales`.`claims_settings` trx id 427C047EA lock_mode X waiting
*** WE ROLL BACK TRANSACTION (1)
```

Also you can look at the transactions section:

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

## Show locked transactions

```sql
SELECT * FROM INFORMATION_SCHEMA.INNODB_LOCK_WAITS;
```

## Info about InnoDb transactions

This query will show current queries, number of locked tables, rows, isolation level etc.

```sql
SELECT * from INFORMATION_SCHEMA.INNODB_TRX;
```

## Show open tables

This query will show names of currently used tables:

```sql
show open tables where In_Use > 0;
```
