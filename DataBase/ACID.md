
A → Atomicity
C → Consistency
I → Isolation
D → Durability

## 1. Atomicity → 
Atomicity means a transaction is all-or-nothing either all its operations succeed, or none are applied. If any part fails, the entire transaction is rolled back to keep the database consistent.

- Commit If the transaction is successful, the changes are permanently applied.
- Abort/Rollback If the transaction fails, any changes made during the transaction are discarded.

## 2. Consistency →
Consistency in transactions means that the database must remain in a valid state before and after a transaction.

- A valid state follows all defined rules, constraints, and relationships (like primary keys, foreign keys, etc.).
- If a transaction violates any of these rules, it is rolled back to prevent corrupt or invalid data.
- If a transaction deducts money from one account but doesn't add it to another (in a transfer), it violates consistency.
		Total before T occurs = 500 + 200 = 700
		Total after T occurs = 400 + 300 = 700

## 3. Isolation → 
Isolation ensures that transactions run independently without affecting each other. Changes made by one transaction are not visible to others until they are committed.

	It ensures that the result of concurrent transactions is the same as if they were run one after another, preventing issues like:

- ****Dirty reads:**** reading uncommitted data
- ****Non-repeatable reads:**** data changes between two reads
- ****Phantom reads:**** new rows appear during a transaction

## 4. Durability → 
Durability ensures that once a transaction is committed, its changes are permanently saved, even if the system fails. The data is stored in non-volatile memory, so the database can recover to its last committed state without losing data.



## Coding to achieve ACID 



