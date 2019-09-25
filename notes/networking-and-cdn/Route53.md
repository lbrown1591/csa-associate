# Route53

[Back to Index](../../README.md)

Route53 is for DNS and domain management. Its name comes from Route 66 / port 53 (DNS).

## Exam Tips / Summary

- ELBs DO NOT have pre-defined IPv4 address. You need to resolve them using a DNS name.
- Difference between Alias and CNAME:
    - Alias: Can be used for apex record/domain (no subdomain)
    - CNAME: cannot
    - If given the choice on the exam, ALWAYS select the Alias record over a CNAME.
- Common DNS types:
    - Start of Authority (SOA) records
    - NS records
    - A records
    - CNAME records
    - MX records (mail)
    - PTR records (look up a name against IP - opposite of A record)
- You can buy domains straight through AWS
    - Can take up to 3 days to register
- ROUTING:
    - Simple
        - One record, multiple IPs
        - If you specify multiple values in a record, they are all returned in a random order
    - Weighted
        - You can split traffic based on assigned weights
        - For example 10% to us-east-1 and 90% to us-west-1
    - Latency
        - Allows you to route your traffic based on lowest latency for end user
        - Latency record set for each ELB/EC2 resource
    - Failover
        - Active/passive (primary/DR) setup using health check
        - If health check fails, it will failover to the other record
    - Geolocation
        - Lets you route based on users' geolocation
    - Geoproximity (Traffic Flow Only)
        - Routes traffic based on users' and your resources' geographic locations
        - You can send more or less traffic to a given resource by specifying a 'bias'
            - This grows or shrinks the size of a geographic location from which traffic is routed to a resource
        - MUST use Route53 'Traffic Flow'
    - Multivalue Answer
        - Similar to simple
        - Difference: can enable health check for each record set
        - Supports failover if a health check does not pass

## DNS 101

Used to convert human-friendly domain names into an Internet Protocol (IP) address.

- IPv4 addresses are running out
    - 32 bit field (over 4 billion addresses)
- IPv6 created to address this
    - 128 bit address space
    - 340 undecillion addresses
- .com.au
    - .com is a top-level domain
    - .au is the second-level domain
    - Controlled by IANA - Internet Assigned Numbers Authority
- ICANN enforces uniqueness of all domains using InterNIC
- SOA (Start of Authority) record stores information about...
    - Name of the server
    - Admin of the zone
    - Current version of data file
    - \# seconds TTL on resource records
- NS: Name Server Records
    - Used by TLD servers to direct traffic to the content DNS server
- DNS Records:
    - A: "Address record" used to translate a domain to IP address
    - CNAME: Canonical Name used to translate one domain to another domain
    - ALIAS: Used to map resource record sets in hosted zone to ELB, CloudFront Dists, or S3 buckets configured as websites
        - Looks similar to a CNAME record, because you can map `www.example.com` to `abc1234.elb.amazonaws.com`
        - Difference - CNAMEs can't be used for naked domain names, i.e. `acloud.guru`. Must be an A record or ALIAS.
- TTL: The length a DNS record is cached on the resolving server or local PC
    - The lower TTL, the faster DNS changes are updated