# DynamoDB

[Back to Index](../../README.md)

## Exam Tips / Summary

- Stored on SSD
- 3 geographically distinct data centers
- Eventually consistent reads
    - Usually within a second. Repeating a read after a short time should return updated data.
- Strongly consistent reads (1 SECOND RULE!)
    - Will return a result reflecting all writes that have received a successful response prior to the read.
    - For cases where you need < 1 second read consistency

## What is it?

A fast, flexible, super low-latency (single millisecond) NoSQL DB service.

Fully managed, and supports both document and key-value data models.

Great for mobile, web, gaming, ad-tech, IoT, and many other applications.

## Basics

- Stored on SSD
- 3 geographically distinct data centers
- Eventually consistent reads
    - Usually within a second. Repeating a read after a short time should return updated data.
- Strongly consistent reads (1 SECOND RULE!)
    - Will return a result reflecting all writes that have received a successful response prior to the read.
    - For cases where you need < 1 second read consistency
