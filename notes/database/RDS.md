# RDS: Relational Database Service

[Back to Index](../../README.md)

## Exam Tips / Summary

- DB Flavors:
    - SQL Server
    - Oracle
    - MySQL
    - PostgreSQL
    - Amazon Aurora
    - MariaDB
- RDS runs on virtual machines
- You cannot log into these operating systems
- Patching of the RDS OS and DB is Amazon's responsibility
- RDS is NOT serverless, you just can't access the server
    - Aurora serverless is the exception
- 2 types of backups
    - Automated backups
    - DB snapshots
- Read Replicas
    - Can be multi-AZ
    - Used to increase performance
    - Must have backups turned on
    - Can be in different regions
    - Can be Aurora or MySQL
    - Can be promoted to master - this will break the replica
- Encryption
    - MySQL, Oracle, SQL Server, PostgreSQL, MariaDB, Aurora
    - Uses AWS KMS
    - Once your RDS instance is encrypted, so are read replicas, backups, and snapshots
- You can force failover in multi-AZ by rebooting the RDS instance

## Features

### Backups

Restored RDS instances will be new instances w/ new endpoints.

#### Automated

This allows you to recover your database to any point in time within a retention period between 1-35 days.

- Takes full daily snapshots
- Stores transaction logs throughout the day
- This allows you to do point-in-time recovery down to the second
- Enabled by default
- Stored in S3. You get free storage space equal to the size of your DB
- Backups are taken within a defined window. Storage I/O may be suspended during this time, so you may experience latency

#### Snapshots

These are done manually by the user.

They are stored even after deletion of the RDS instance, unlike automated backups.

### Encryption

Encryption at rest is supported (via KMS) by:
- MySQL
- Oracle
- SQL Server
- PostgreSQL
- MariaDB
- Aurora

Once encrypted, the underlying storage is encrypted along with automated backups, read replicas, and snapshots.

### Multi-AZ

DNS points to 1 of 2 copies. If one goes out, DNS will update to point to the replica automatically. Failover is automatic.

- An exact copy of your prod database in another AZ, replicated synchronously
- In the case of maintainance, DB failure, or AZ failure, RDS will automatically failover to the other endpoint

Supported for everything but Aurora (Aurora is completely fault tolerant by design)

### Read Replicas (For Performance)

Failover is not automatic. If the primary database fails, or gets a burst of traffic, you can scale out to read-only copies of the database (up to 5).

You can have read replicas of your read replicas, and you can promote your read replicas to a first class database.

- Used for scaling, NOT DR
- Must have automatic backups turned on in order to deploy a read replica
- You can have up to 5 copies of any database
- You can have read replicas of read replicas (watch out for replication latency)
- Each read replica has its own endpoint
- You can have multi-AZ read replicas
- You can create read replicas of a multi-AZ source database
- They can be promoted to be their own database, breaking replication
- You can have a read replica in a second region