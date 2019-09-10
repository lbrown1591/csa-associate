# CloudWatch

[Back to Index](../../README.md)

CloudWatch is all about performance monitoring.

## LOOK OUT

AWS likes to put trick questions to confuse you with CloudTrail. **CloudTrail** is basically a CCTV for your cloud. It monitors actions through the console and APIs, including users, accounts, IPs, and times.

## Exam Tips & Summary

- Used for monitoring performance
- Can monitor most of AWS as well as applications running on AWS
- CloudWatch with EC2 will monitor events every 5 min. by default
    - You can have 1 minute intervals by turning on 'detailed monitoring'
- You can create CloudWatch alarms which trigger notifications
- CloudWatch is all about performance, while CloudTrail is all about auditing

## What can it monitor?

- Compute
    - EC2 instances
    - Autoscaling groups
    - Elastic load balancers
    - Route53 health checks
- Storage and CDN
    - EBS Volumes
    - Storage Gateways
    - CloudFront

## Host-Level Metrics Consist Of...

- CPU
- Network
- Disk
- Status check