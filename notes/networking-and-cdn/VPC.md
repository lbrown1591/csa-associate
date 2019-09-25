# VPCs

[Back to Index](../../README.md)

The day before or the day of your exam, build out a VPC from memory, and you should be able to pass.

VPC lets you provision a logically isolated section of the AWS cloud where you can launch resources in a virtual network you define.

## Exam Tips / Summary

- 5 VPCs per region
- VPN connection consists of
    - Customer Gateway
    - Virtual Private Gateway
- Gives you complete control over virtual networking environment
    - IP address range
    - subnets
    - route tables
    - network gateways
- Example
    - Public facing subnet for webservers with access to the internet
    - Private subnet with no internet access for backend systems, DBs, and application servers
- Security
    - Security groups
    - NAC lists
- Supports Hardware VPN, so you can connect your corporate data center to a VPC
- **Bastion Host**: An EC2 host in a public subnet that can SSH into a private subnet's EC2 instance
- Reserved private ranges:
    - 10.0.0.0-10.255.255.255 (10/8 prefix)
    - 172.16.0.0-172.31.255.255 (172.16/12 prefix)
    - 192.168.0.0-192.168.255.255 (192.168/16 prefix)
- Default vs. Custom
    - Default
        - User friendly, immediately deploy instances
        - All subnets have a route to the internet
        - Each EC2 instance has a public and private IP address
- VPC Peering
    - Connect VPCs via direct network route using private IP addresses
    - Instances behave as if they were on the same private network
    - You can peer VPCs with other AWS accounts as well as other same-account VPCs
    - Star configuration (1 central with 4 others) - NO TRANSITIVE PEERING (You can't peer THROUGH a VPC to another)
    - You can peer across regions
- Think of a VPC as a logical datacenter in AWS
- Consists of...
    - IGWs (Virtual Private Gateways)
    - Route Tables
    - Network Access Control Lists
    - Subnets
    - Security Groups
- 1 subnet = 1 availability zone
    - You cannot have a subnet that reaches acrossed multiple AZs
    - You CAN have multiple subnets in 1 AZ
- Security groups are stateful, while ACLs are stateless (you can add allow and deny rules, and rules are NOT automatically mirrored)
- NO TRANSITIVE PEERING THROUGH VPCs
- When you create a VPC, a default Route Table, NACL, and default security group is created
    - No subnets or IGWs are created
- AZs are randomized per account
- Amazon always reserves 5 IPs in your subnets
- Only 1 IGW per VPC
- NAT Gateways/Instances
    - Used to provide internet traffic to EC2 instances in a private subnet
- NAT Instances
    - When creating, disable source/destination checks
    - NAT instances must be in a public subnet
    - There must be a route out of the private subnet to the NAT instance
    - Amount of traffic it supports depends on the instance size
    - Can create HA using...
        - Autoscaling groups
        - Multiple subnets in different AZs
        - Script to automate failover
    - Always behind a security group
- NAT Gateways
    - Redundant inside AZ
    - Only 1 inside 1 AZ
    - Preferred by enterprise
    - Starts at 5Gbps, scales to 45Gbps
    - No need to patch
    - Not associated with security groups
    - Automatically assigned public IP address
    - Update your route tables
    - No need to disable source/destination checks
    - If multiple AZs depend on 1 NAT gateway, and that gateway goes out, the AZ resources depending on the gateway will lose internet access
        - To create an AZ-independent architecture, use a NAT gateway per AZ
- NACLs
    - Rules are evaluated in numerical order
    - If you have a DENY rule that comes AFTER YOUR ALLOW rule, the traffic will be allowed
    - Multiple subnets can be associated with a NACL, but a subnet can only be associated with one NACL
    - Contain a numbered list of rules
        - Evaluated in order, starting with the lowest numbered rule
        - Network ACLs have separate inbound and outbound rules, and each rule can 
    - Evaluated BEFORE security groups
    - VPC comes with a default ACL allowing all outbound and inbound traffic
    - Custom ones start out by denying all traffic until you add rules
    - Each subnet in a VPC must be associated with a network ACL
        - If you don't explicitly associate a subnet with a network ACL, the subnet is automatically associated with the default network ACL
    - Block IPs using NACLs, NOT security groups
    - Stateless: responses to allowed inbound traffic are subject to the rules for inbound traffic, and vice-versa
- Direct Connect
    - Directly connects your data center to AWS
    - Useful for high-throughput workloads (i.e. lots of network traffic)
    - Useful if you need a stable and reliable secure connection
- VPC Endpoints
    - Connect private subnet to AWS resources without NAT, direct connect, VPN, or IGW
    - Interface endpoints
    - Gateway endpoints
        - S3
        - DynamoDB

## What can you do with VPC?

- Launch instances into subnets of your choosing
- Assign custom IPs in each subnet
- Configure route tables between subnets
- Create internet gateway and attach it to a VPC
- Much better security control over AWS resources
- Instance security groups
- Subnet network ACLs

## Components of a VPC

### IGWs (Virtual Private Gateways)
### Route Tables
### Network Access Control Lists
### Subnets
### Security Groups

## NAT Instances vs. NAT Gateways

NAT gateways will usually be used, but instances are still on the exam. They are highly available and not tied to a single EC2 instance.

If you have a PRIVATE subnet that needs to communicate with the internet, you can...
- Create a NAT gateway or instance in a public subnet
- Add a route from your private subnet pointing to your NAT gateway/instance

## VPC Flow Logs

- Capture information about IP traffic
    - VPCs
    - Subnets
    - Network interface level (ENIs)
- You cannot enable flow logs for VPCs peered with yours unless the peered VPC is in your own account
- You cannot tag a flow log
- After you've created a flow log, you can't change its configuration
- Not all IP traffic is monitored
    - Traffic generated by instances when they contact the Amazon DNS server
    - If you use your own DNS server, all traffic to that DNS server is logged
    - Traffic generated by a Windows instance for Amazon Windows License activation
    - To and from 169.254.169.254
    - DHCP traffic
    - Traffic to reserved IP addresses for default VPC router

## Bastion Hosts

- Used to securely administer EC2 instances using SSH or RDP (called Jump Boxes in Australia)
- Special purpose computer on network specifically designed/configured to withstand attacks
- Only hosts a single application - all others limited or removed to withstand attacks
- Hardened due to location and purpose
    - Outside of a firewall
    - In a demilitarized zone (DMZ), or public subnet
    - Usually involves access from untrusted networks or computers
- There are bastion hosts on the community images page
- You can NOT use a NAT Gateway as a bastion host

## Direct Connect

- Cloud service solution that makes it easy to establish a dedicated network connection from your premises to AWS
- Can establish private connectivity between AWS and your datacenter, office, or colocation environment
- Can reduce network costs, increase bandwidth throughput, and provide more consistent network experience than internet based connections

## VPC Endpoints

Enables you to connect your VPC to supported AWS services powered by PrivateLink without an IGW, NAT device, VPN, or AWS Direct Connect connection.

Instances in your VPC do not require public IP addresses to communicate with resources in the service. Traffic does not leave the Amazon network.

#### Interface Endpoints

An Elastic Network Interface (ENI) with a private IP address that serves as an entry point for traffic destined to a supported service.

#### Gateway Endpoints

Look just like NAT gateways.

Supported for S3 and DynamoDB.