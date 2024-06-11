# AWS - Developer Associate Certificate

Domains of the course [Course-Link, PluralSight](https://d1.awsstatic.com/training-and-certification/docs-dev-associate/AWS-Certified-Developer-Associate_Exam-Guide.pdf)

1. Development of aws services (32%)
2. security (26%)
3. deployment (24%)
4. troubleshooting and optimization (18%)


130 minutes - 65 questions

## IAM

IAM - Identity Access Management

main usage - to controll acess to the resources
end points - users, groups , roles , policies

- roles - defines a set of conditions on resources , users or group or applications inherit these properties
- A role provides a dynamic way to provide permission of interact with the resoruces
- policies - A direct way to interact way with resources.

A role can follow a set of policies to interact with

E.g - creating a new policy and attaching a role to it

- policy name - `ReadOnlyAccessToS3Bucket`
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::example-bucket",
                "arn:aws:s3:::example-bucket/*"
            ]
        }
    ]
}

```

- Role Name: EC2ReadOnlyS3Role

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": "ec2.amazonaws.com"
            },
            "Action": "sts:AssumeRole"
        }
    ]
}

```

To check if a policy can perform a set of actions we can use

- [Policy Sim](https://policysim.aws.amazon.com)

## EC2

- Elastic Cloud Compute

- pricing types

1. on Demand (pay per use)
2. Reserved (prepay and long term)
3. spot (fix a max price , the instances are balanced dynamically)
4. dedicated

EBS storage types

1. gp2 , gp3 - general purpose (3000 - 16000 IOPS)
2. io1,io2,SAN - faster access (16000 - 64000 IOPS)
3. SAN - (256000 IOPS)
4. st1 (500 mbps), sc1 (250mbps) - hdd storage

- ec2 instance and ebs volume should be in the same availability zone in order to attach them.

## Elastic Load Balancer

types

1. application (http / https requests)
2. network load (tcp)
3. gateway load balancers (used to serve third party applications such as firewalls)

- Main use case is to efficiently forward requests to the appropriate server
- the load balancer has a private address by default , it used the x-forwarded-by property to find the ip address of end user

![alt text](aws1.png)

## RDS

- main use case: In case of Transaction data , use RDS if use case need to perform analysis on large data use RedShift

to improve performance - Read replica snapshots of rds instances are provided
to resolve disasters - Multiple rds instances are allocated in different Availability zones


- two different options to perform backups

1. automated - handled by aws , these are performed in definitive time window, can be stored across multiple time zones

2. manual - a snapshot is created manually by the user and its stored in an s3 bucket (charged) , this snapshot would be having a different RDS endpoint compared to the parent

- An unencrypted RDS instance can be encrypted by making a manual snaphot and encrypting it.

![alt text](aws2.png)

- RDS proxy: its used to load balance all the incoming traffic to the rds instance , in case of any instance failures all the transactions are stored in the proxy servers and are resolved post failure resolving process.


## ElastiCache

In memory storage in aws, two types are available

1. memcache (used to store simple cache , session storage of website) - memcache sits in front of an RDS instance
2. Redis (complex datatypes, larger applications such as online gaming)

MemoryDB for redis is alternative to an RDS service

## Parameter Store

System manager service provides the parameter store to encrypt sensitive data and pass on to other resources

## Secrets Manager

- AWS secret manager is used to store all the database passwords , api keys . etc.
- rotation of the keys can be enables in secrets manager which is handled by an lambda function
- use programmatic way to retrive keys from the manager
- it uses a key from aws kms to encrypt and it can be replicated in other zones

## EC2 Image Builder

- EC2 instances can be used to test and build images

![alt text](aws3.png)

## S3

Simple Storage Service

Different types

1. Standard (general purpose)
2. standard - infrequent access (store backup files)
3. glacier and glacier archive (large archive data)
4. Intelligent Tiering (auto shift between storage types based on access frequency)

Encryption types

1. SSE-S3 (managed by s3 , not by kms - default)
2. SSE-KMS (managed by kms)
3. SSE-C (managed by customer)
4. DSSE-KMS (dual encryption managed by kms)

By default CORS is disabled between two s3 buckets , edit the properties and allow the host to enable CORS

## CloudFront

CDN network provided by aws

1. Edge Location (cache location with definitve ttl)
2. CloudFront origin (origin of files (s3,ec2,elb,r53))

- Can assign a resource to be restricted by use of signed URL or signed cookies.

additional topics - AWS WAF (firewall) --> mainly used to restrict all the incoming traffic to the origin

- A new origin access identity is required whenever we need to restrict access to the s3 buckets

### Allowed Methods

1. GET, HEAD (resource_headers) (read-only)
2. GET, HEAD, OPTIONS (request_to_know_allowed_methods) (read-only)
3. GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE (read-only and write)

