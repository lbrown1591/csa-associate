# Identity and Access Management (IAM)

[Back to Index](../../README.md)

## Exam Tips / Summary

- New users have **no permissions** when they are first created.
- New users are assigned an **Access Key ID** and **Secret Access Key** when first created.
- These are NOT the same as a password.
- You can only view the secret access key once.
- ALWAYS enable MFA on the root account.
- You can customize password requirements and rotation/expiration policies.

## What is it?

A global service (actions affect all regions, not just one) which allows you to manage users and their respective access levels to various AWS resources programatically and through the console.

## Features

- Centralized control of AWS account
- Shared access to your AWS account
- Granular permissions
- Identity federation (Google, Facebook, etc.)
- MFA
- Temporary access for users, devices, and services when necessary
- Password rotation policies
- Integrates with many AWS services
- PCI DSS compliance

## Key Terminology

**Users**: End users, people, members of the organization.

**Groups**: A collection of users. Users in a group will inherit the permissions of the group.

**Roles**: Collections of policies which can be assigned to users, groups, and AWS resources.

**Policies**: Made up of JSON documents, known as **policy documents**. They give permissions indicating what a user/role/group is able to do in AWS.

**Root Account**: The account created when you sign up. It has full admin access.