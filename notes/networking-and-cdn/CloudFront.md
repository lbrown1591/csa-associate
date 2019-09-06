# Cloudfront

[Back to Index](../../README.md)

Cloudfront is a content delivery network (CDN), meaning it's a collection of servers that deliver content to users efficiently based on geographic location.

## Exam Tips / Summary

- Data is stored at **Edge Locations**.
- **Origins** are the origin of all the files that the CDN will distribute. This could be an S3 bucket, EC2 instance, Elastic Load Balancer, or Route53.
- **Distributions** are the name given to the CDN, consisting of Edge Locations.
- Can be used to deliver **dynamic, static, streaming, and interactive** content with the best possible performance, from the closest geographic location.
- Edge locations are read/write capable
- Types 
    - Web Distributions for Websites
    - RTMP for Streaming
- Objects are cached for the life of the TTL (Time to Live)
- You can invalidate cached content, but you will be charged

## Types

**Web Distributions**: Typically used for websites.

**RTMP**: Used for media streaming.