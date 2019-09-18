# Aurora

[Back to Index](../../README.md)

## Exam Tips / Summary

- Relational DB compatible with MySQL and PostgreSQL
- Up to 5x better performance than MySQL
- Price point 1/10th of a commercial DB, with similar performance/availability
- Starts at 10GB, scales in 10GB increments up to 64TB (Storage Autoscaling)
- Compute resources can scale to 32 vCPUs and 244 GB of memory
- 2 copies of your data in EACH AZ, with a minimum of 3 AZs
    - 6 copies of data.
    - Only available in regions with 3 AZs
- Designed to transparently handle loss of up to 2 copies w/o affecting write, 3 without affecting read
- Storage is self-healing (blocks and disks)
- 2 types of replicas
    - Aurora (currently 15)
    - MySQL Read replicas (currently 5)
- Backups always enabled without affect on performance
- Snapshots do not impact performance
- You can share aurora snapshots with other AWS accounts