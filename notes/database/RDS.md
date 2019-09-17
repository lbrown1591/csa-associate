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

## Features

### Multi-AZ

DNS points to 1 of 2 copies. If one goes out, DNS will update to point to the replica automatically. Failover is automatic.

### Read Replicas (For Performance)

Failover is not automatic. If the primary database fails, or gets a burst of traffic, you can scale out to read-only copies of the database (up to 5).