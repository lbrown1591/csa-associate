# Storage Gateway

This is a service that connects an on-premis software appliance with cloud-based storage for cost and scalability.

## Exam Tips / Summary



## Installation

AWS Storage Gateway's software is available for **download as a VM** to be used under the VMware ESXi or Microsoft Hyper-V hypervisor.

## Types

### File Gateway (NFS & SMB) - A way to store your files in S3
- Files are stored as objects in S3
- Accessed through a NFS (Network File System) mount point
- Ownership, permissions, and timestamps are stored in S3 user-metadata
- Once transferred to S3, objects can be managed as native S3 objects, with all of the benefits of S3 (versioning, lifecycle mgmt., CRR)

### Volume Gateway (iSCSI) - A way to store a copy of your HDD
- Stored volumes
    - 1GB - 16GB
    - Store primary data locally, backing up to AWS
    - Low-latency access to **entire dataset** with durable off-site backups
    - Backups are EBS snapshots
        - These snapshots only capture changed blocks
        - Compressed to minimize storage charges
- Cached volumes
    - Doesn't do entire dataset locally
    - Only most frequently accessed data
    - Up to 32TB in size
    - Stores data in S3, and gives local access to frequently accessed data

### Tape Gateway (Virtual Tape Library) - Cloud solution for tape-based archiving
- Durable, cost-effective solution to archive tape-based data in AWS