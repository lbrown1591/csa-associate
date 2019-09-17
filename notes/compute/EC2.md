# EC2: Elastic Compute Cloud

[Back to Index](../../README.md)

EC2 is a web service providing resizable computing capacity in the cloud.

## Exam Tips / Summary

- Types:
    - On Demand
    - Reserved
    - Spot
    - Dedicated Hosts
- If Amazon terminates a Spot instance, you are not charged for that hour. If you terminate it, you are charged for any hour in which it ran.
- Termination protection is **turned off** by default. You must explicitly turn it on
- On an EBS-backed instance, the root volume will be deleted by default upon termination of the instance, but additional volumes will persist
- EBS Root Volumes of default AMIs cannot be encrypted - you can use third party tools or your own AMIs to encrypt the root volume
- Additional volumes beyond the root **can** be encrypted
- Security Groups
    - Changes are immediate
    - They control inbound and outbound internet access
    - All inbound blocked by default
    - All outbound allowed
    - Any number of instances can be tied to a security group
    - Multiple security groups can be applied to EC2 instances
    - Security groups are stateful (if you create an inbound rule, that traffic is also allowed out)
    - You cannot block specific IPs, instead use NACLs
    - You can specify 'allow' rules, but not 'deny' rules
- EBS (Elastic Block Store)
    - Persistent block storage volumes for use with EC2
    - Each EBS volume is replicated within its AZ to protect from component failure
    - Will be in the same AZ as the attached EC2 machine
    - Types:
        - General Purpose (SSD)
            - API name: gp2
            - Max IOPS/volume: 16,000
        - Provisioned IOPS (SSD)
            - API name: io1
            - Max IOPS/volume: 64,000
        - Throughput Optimised HDD
            - API name: st1
            - Max IOPS/volume: 500
        - Cold Hard Disk Drive
            - API name: sc1
            - Max IOPS/volume: 250
        - Magnetic
            - API name: Standard
            - Max IOPS/volume: 40-200
- Volumes
    - Exist on EBS
- Snapshots
    - Exist on S3
    - Point in time copies of volumes
    - Incremental - only blocks that have changed will be stored
    - First snapshot may take time to create
    - Snapshots of root volumes should be done while instance is stopped for data consistency, but can be done while running
- AMI (Amazon Machine Images) can be created from volumes or snapshots
- EBS volume size can be changed on the fly - size and storage type
- EC2 volumes can be moved between availability zones - Snapshot => AMI => Launch in new AZ
    - Between regions: Snapshot => AMI => Copy to destination region => Launch in new region
- Instance Store
    - Ephemeral data that is deleted when instance is stopped
    - If underlying host fails, data is lost
- EBS-backed Instances
    - Can be stopped and rebooted
    - The above will fix underlying system issues (hypervisor issues)
    - You can tell AWS to keep EBS ROOT volumes on termination
- The `User Data` field when launching an instance allows you to add a boostrap script beginning with `#!/bin/bash` that will run as root upon launch
- Get instance metadata with curl http://169.254.169.254/latest/meta-data
- Get instance user data with curl http://169.254.169.254/latest/user-data
- Placement groups
    - Only certain types of instances can be launched in a placement group
        - Compute optimized
        - GPU
        - Memory optimized
        - Storage optimized
    - AWS recommends homogenous instances within clustered placement groups
    - You can't merge placement groups
    - You can't move an existing instance directly into a placement group
    - **Clustered placement**
        - Low network latency, high network throughput
        - Cannot span multiple AZs
    - **Spread placement**
        - Individual critical instances (separate racks)
        - Can span multiple AZs
    - **Partitioned placement**
        - Similar to spread, but multiple instances per partition (clusters of instances on separate racks)
        - Can span multiple AZs

## Placement Groups

### Clustered

A grouping of instances in a single AZ, that have low network latency, high network throughput, or both.

### Spread

A group of instances that will be on separate physical racks.

### Partitioned

Very similar to spread, but you can have multiple instances within a partition. Each partition is on a separate rack.

## Pricing Models

### On Demand

Allows you to pay a fixed rate by the hour (or by the second) with no commitment.

- Low cost and flexibility w/o upfront payment or long-term commitment
- Good for short-term, spiky, or unpredictable workloads that cannot be interrupted
- Applications being developed or tested on EC2 for the first time

### Reserved

Capacity reservation, offering significant discount of the typical hourly rate. Terms of 1 or 3 years.

- Good for steady state/predictable usage
- Applications that require reserved capacity
- Upfront payments to reduce total computing costs even further

#### Types of Reserved Instances

- **Standard**: Offer up to 75% off on demand instances. Greater discount with more up-front payment and longer contracts.
- **Convertible**: Offer up to 54% off on-demand, capability to change between instance types.
- **Scheduled Reserved**: Available to launch within time windows you reserve.

### Spot

Excess capacity, allowing you to bid whatever price you want. Great for flexible start/end times.

- Applications w/ flexible start and end times
- Applications that are only feasible at very low compute prices
- Users with urgent computing needs for large amounts of additional capacity

### Dedicated Hosts

Physical EC2 server dedicated to your use, useful for strict license conditions.

- Useful for regulatory req.'s that may not support multi-tenant virtualization
- Great for licensing that doesn't support multi-tenancy or cloud deployments
- Can be purchased on-demand (hourly)
- Can be purchased as a reservation for up to 70% off the on-demand pricing

## Security Groups

These control inbound and outbound network access to an EC2 instance.

- Changes are immediate
- All inbound blocked by default
- All outbound allowed
- Any number of instances can be tied to a security group
- Multiple security groups can be applied to EC2 instances
- Security groups are stateful (if you create an inbound rule, that traffic is also allowed out)
- You cannot block specific IPs, instead use NACLs
- You can specify 'allow' rules, but not 'deny' rules

## EBS (Elastic Block Store)

EBS provides persistent block storage volumes for use with EC2.

- Each EBS volume is replicated within its AZ to protect from component failure
- Will be in the same AZ as the attached EC2 machine
- Types:
    - General Purpose (SSD)
        - API name: gp2
        - Max IOPS/volume: 16,000
    - Provisioned IOPS (SSD)
        - API name: io1
        - Max IOPS/volume: 64,000
    - Throughput Optimised HDD
        - API name: st1
        - Max IOPS/volume: 500
    - Cold Hard Disk Drive
        - API name: sc1
        - Max IOPS/volume: 250
    - Magnetic
        - API name: Standard
        - Max IOPS/volume: 40-200

## Volumes and Snapshots

### Volumes
- Exist on EBS
- To encrypt a ROOT volume after creation:
    - Create snapshot
    - Copy snapshot, and check 'Encrypt this Snapshot'
    - Create an image from the encrypted snapshot
- Volumes restored from encrypted snapshots are encrypted automatically
- You can encrypt ROOT device volumes upon creation of the EC2 instance

### Snapshots
- Exist on S3
- Point in time copies of volumes
- Incremental - only blocks that have changed will be stored
- First snapshot may take time to create
- Snapshots of root volumes should be done while instance is stopped for data consistency, but can be done while running
- Snapshots of encrypted volumes are encrypted automatically
- You can share snapshots only if they are unencrypted
- These snapshots can be shared with other AWS accounts or made public

## Amazon Machine Images (AMIs)

You can select your AMI based on:

- Region
- OS
- Architecture (32/64-bit)
- Launch Permissions
- Storage for the Root Device (Root Device Volume)
    - Instance Store (EPHEMERAL STORAGE)
    - EBS-backed volumes
    - All AMIs are one of these

### Types

EBS
- Root device is an Amazon EBS volume based on an Amazon EBS snapshot
- If there are 'System Status Check' issues, they can be resolved by stopping and starting
    - This starts the machine under a new hypervisor
    - You CANNOT do this on an Instance Store backed machine

Instance Store:
- Root device is an instance store volume created from a template stored in S3
