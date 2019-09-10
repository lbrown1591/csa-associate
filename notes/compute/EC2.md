# EC2 (Elastic Compute Cloud)

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