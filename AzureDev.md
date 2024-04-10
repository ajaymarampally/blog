# Azure Developer Associate

`Credits` - [Microsoft Learn](https://learn.microsoft.com/en-us/credentials/certifications/azure-developer/?ns-enrollment-type=Collection&ns-enrollment-id=e21nurz5wk3m71&practice-assessment-type=certification)

## Table of contents

|  Concept     | Primary Usages  |
| ---------------- | -------------------- |
| AZ:900: Azure Fundamentals | Introduction to Azure Fundamentals |
| AZ-204: Implement Azure App Service Web Apps |


### Azure Fundamentals

## shared responsibility model

In this model, the responsibilites are shared between the cloud provider and the client, for instance in case of cloud SQL server, the provider is responsible for setting up the instances whereas the client is responsible for data ingestion and providing access.

Different Cloud Service types

```
1. Infrastructure as a Service (IAAS) - client is most responsible - Lift and shift
2. Platform as a Service (PAAS) - middle ground - Development Kits
3. Software as a Service (SAAS) - cloud provider is most responsible - Email Service
```

![alt text](az1.png)

## Vertical Scaling

The ability to add more compute power, in case of app development adding more cpu power is vertical scaling

## Horizontal scaling

The ability to add more machines or containers to support the demand(either auto or manual)


### AZ:204 - Azure App service

## Azure App service


```
HTTP-based service for hosting web application , REST API's and back ends. Runs on both linux and windows environments.
```

Components of the App service

1.Operating System (Windows, Linux)
2.Region (West US, East US, etc.)
3.Number of VM instances
4.Size of VM instances (Small, Medium, Large)
5.Pricing tier (Free, Shared, Basic, Standard, Premium, PremiumV2, PremiumV3, Isolated, IsolatedV2)

