# Transaction

## What is transaction ?

It is a collection of queries, which is logically doing a single task or one unit of task. 

> Transaction Lifespan \
> Transaction BEGIN \
> Transaction COMMIT \
> Transaction ROLLBACK \
> Transaction unexpected ending = ROLLBACK crash

Consider a Money transfer transaction example. It includes three sql queries
- select
- update 
- update

First select query checks weather the sender has enough money, then update query deducts the amount from the sender and third update query will update the reciever bank account.

| Account Id | Amount |
|------------|--------|
| 1 | 1000
| 2 |  300

Send 100 from account_id 1 to account_id 2
> BEGIN TX1 \
> Select amount from AccountTable where account_id = 1 \
> amount >= 100
> update AccountTable set amount = amount - 100 where amount_id = 1\
> update AccountTable set amount = amount + 100 where amount_id = 2 \
> COMMIT TX1

This is logically a single unit of work.

> <b>Just think </b>\
> How database update queries are written to the disk.
> - it is like the data base changes are written to the memory at first and after sometime it is written back to disk
> - Or for each queries it keep on updating the disk.

There is a prone and cause to both the cases in 
- case 1 Rollback are faster.
- case 2 commits are faster.

Commits in postgres are fastest, It optimises on commits. If commits are faster crash are usually less.

## Nature of Transaction
- Usually transaction are to write and update the databases.
- However it is perfectly normal to have a read only transaction.
Example you want to generate a report and you want to get consistent snapshot based on time.

