# AWS - Developer Associate Certificate

Domains of the course [Course-Link](https://d1.awsstatic.com/training-and-certification/docs-dev-associate/AWS-Certified-Developer-Associate_Exam-Guide.pdf)

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

