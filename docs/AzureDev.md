# Azure Developer Associate

`Credits` - [Microsoft Learning](https://learn.microsoft.com/en-us/training/courses/az-204t00?ns-enrollment-type=Collection&ns-enrollment-id=e21nurz5wk3m71#course-syllabus)

## Table of contents

| Concept                                      | Primary Usages                     |
| -------------------------------------------- | ---------------------------------- |
| AZ:900: Azure Fundamentals                   | Introduction to Azure Fundamentals |
| AZ-204: Implement Azure App Service Web Apps |                                    |

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

1. Components of the App service

   1.Operating System (Windows, Linux)
   2.Region (West US, East US, etc.)
   3.Number of VM instances
   4.Size of VM instances (Small, Medium, Large)
   5.Pricing tier (Free, Shared, Basic, Standard, Premium, PremiumV2, PremiumV3, Isolated, IsolatedV2)
2. Deployment

   - Azure DevOps Services: You can push your code to Azure DevOps Services, build your code in the cloud, run the tests, generate a release from the code, and finally, push your code to an Azure Web App.
   - GitHub: connect production branch to directly make changes to App service.
   - Bitbucket: With its similarities to GitHub, you can configure an automated deployment with Bitbucket.

   Manual:

   - CLI: webapp up is a feature of the az command-line interface that packages your app and deploys it. Unlike other deployment methods, az webapp up can create a new App Service web app for you if you haven't already created one.
   - Zip deploy: Use curl or a similar HTTP utility to send a ZIP of your application files to App Service.
   - FTP/S: FTP or FTPS is a traditional way of pushing your code to many hosting environments, including App Service.

   We can use deployment swaps method to save time , in this method we deploy to staging and swap the production build.

## Security

### Azure App service Authentication providers

| Provider                    | Sign-in endpoint          | How-To guidance                               |
| --------------------------- | ------------------------- | --------------------------------------------- |
| Microsoft identity platform | /.auth/login/aad          | App Service Microsoft identity platform login |
| Facebook                    | /.auth/login/facebook     | App Service Facebook login                    |
| Google                      | /.auth/login/google       | App Service Google login                      |
| Twitter                     | /.auth/login/twitter      | App Service Twitter login                     |
| Any OpenID Connect provider | /.auth/login/providername | App Service OpenID Connect login              |
| GitHub                      | /.auth/login/github       | App Service GitHub login                      |

### how it works

Both the authetication and Authorization modules run in same sandbox, before the http request hit the App service they are processed.services provided by this module are

- validate, store and refresh OAuth tokens provided by identity providers
- Manage the authenticated sessions
- inject identity information into http headers

### Authentication Flow

| Step                            | Without provider SDK                                                                             | With provider SDK                                                                                                                               |
| ------------------------------- | ------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| Sign user in                    | Redirects client to /.auth/login/provider.                                                       | Client code signs user in directly with provider's SDK and receives an authentication token. For information, see the provider's documentation. |
| Post-authentication             | Provider redirects client to /.auth/login/provider/callback.                                     | Client code posts token from provider to /.auth/login/provider for validation.                                                                  |
| Establish authenticated session | App Service adds authenticated cookie to response.                                               | App Service returns its own authentication token to client code.                                                                                |
| Serve authenticated content     | Client includes authentication cookie in subsequent requests (automatically handled by browser). | Client code presents authentication token in X-ZUMO-AUTH header (automatically handled by Mobile Apps client SDKs).                             |

### Two options are provided for authentication:

- without provider sdk: The browser app provides the auth login page to the user, the server code manages the sign in process thus making it server-directed flow
- with provider sdk: The client application signs into the provider manually and submits the token to the App server, this is client-directed flow and mainly used in browser-less apps, RestAPI's, Azure functions

### Authorization Modes

- unauthenticated --> The App Server allows unautheticated traffic to App server, can add a step to inject http headers.
- authenticated --> The App server rejects all the unauth traffic, a redirect is issued to `.\auth\login\<provider>`

### Token Store

App Service provides in-built token store at the init of the application, when authetication is enabled with any provider a default token store is provided.

### Networking

In Standard plans , all the Apps in the service run under the same worker node, similarly all the outbound addresses for the application are shared. All the possible IP's are listed under the `possibleOutboundIPAddresses` property.

in azure shell

```bash
az webapp show \
    --resource-group <group_name> \
    --name <app_name> \
    --query outboundIpAddresses \
    --output tsv
```

### Application Settings

All the environmental variables of the application are controller through appSettings Configuaration file, this files will be used for connecting to Azure MySQL servers in production env. , the contents of web.config and appsettings.json by default are used for development env.

### Security Certificates

A certificate is shared between app services in the same resourceGroup and region combination.

| Option                                        | Description                                                                                                                                                      |
| --------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Create a free App Service managed certificate | A private certificate that's free of charge and easy to use if you just need to secure your custom domain in App Service.                                        |
| Purchase an App Service certificate           | A private certificate that's managed by Azure. It combines the simplicity of automated certificate management and the flexibility of renewal and export options. |
| Import a certificate from Key Vault           | Useful if you use Azure Key Vault to manage your certificates.                                                                                                   |
| Upload a private certificate                  | If you already have a private certificate from a third-party provider, you can upload it.                                                                        |
| Upload a public certificate                   | Public certificates aren't used to secure custom domains, but you can load them into your code if you need them to access remote resources.                      |

All the certifcates purchased through azure are stored in `azure keyValut`

### Metrics for AutoScale

Autoscaling by metric requires that you define one or more autoscale rules. An autoscale rule specifies a metric to monitor, and how autoscaling should respond when this metric crosses a defined threshold. The metrics you can monitor for a web app are:

- CPU Percentage. This metric is an indication of the CPU utilization across all instances. A high value shows that instances are becoming CPU-bound, which could cause delays in processing client requests.
- Memory Percentage. This metric captures the memory occupancy of the application across all instances. A high value indicates that free memory could be running low, and could cause one or more instances to fail.
- Disk Queue Length. This metric is a measure of the number of outstanding I/O requests across all instances. A high value means that disk contention could be occurring.
- Http Queue Length. This metric shows how many client requests are waiting for processing by the web app. If this number is large, client requests might fail with HTTP 408 (Timeout) errors.
- Data In. This metric is the number of bytes received across all instances.
- Data Out. This metric is the number of bytes sent by all instances.

A duration is defined for capturing the Autoscale metrics , the whole duration is divided into equal time grains.

### slot swapping

The following is carried out during swap

- All the settings of the target(production build) are copied into source slot
- HTTP requests are sent to all instances in the source , if a response is recived the slot is considered warmed up.
- Once All the instances are warmed up, the route configurations are swapped to route the traffic to new slot

### Routing

To get user feedback for the new features, a portion of the production traffic can be routed to the new staging slot randomly. The user is pointed to the staging slot until the end of client session. A cookie `x-ms-routing-name=staging` indicates the same.

- Azure WebJob contents --> these are the background workers which process specific tasks on http requests or message queues
- Azure webjob schedulers --> these are background workers which trigger on a scheduled basis mainly used for data synchronization.

## Azure Functions

Azure functions are similar to WebJobs of the App Service , but have more flexible triggers and standalone properties. It is also similar to Azure Logic Apps in terms of functionality but has a different development approach, Logic Apps are design driven where as Functions are code-first architecture.

`functionAppScaleLimit` is used to fix the scale Limit for the Azure Function lies in range(0-max(200))

A function contains two components code and config.json file, A function can have only one trigger.

### Function App

A Function App is a holder for all the functions, all the items share same deployment method and runtime.

Data required for the functions is passed as parameters and the function returns the output. No services are connected in this way.

`dataType` property is used to define the binding type, it's required in interpretted languages, in compiled languages the type is inferred from runtime.

useCase --> whenever a message is added to Azure Queue , add a table row to Azure table Storage.

```
{
  "bindings": [
    {
      "type": "queueTrigger",
      "direction": "in",
      "name": "order",
      "queueName": "myqueue-items",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    },
    {
      "type": "table",
      "direction": "out",
      "name": "$return",
      "tableName": "outTable",
      "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

`type` --> trigger type
`direction` --> data binding direction (in,out)
`name` --> data parameter
`queueName` --> name of queue column (name of the trigger)
`connection` --> connection String

The Functions can also be triggered using class libraries , in this case the function.json file is not required.

e.g

```c#
public static class QueueTriggerTableOutput
{
    [FunctionName("QueueTriggerTableOutput")]
    [return: Table("outTable", Connection = "MY_TABLE_STORAGE_ACCT_APP_SETTING")]
    public static Person Run(
        [QueueTrigger("myqueue-items", Connection = "MY_STORAGE_ACCT_APP_SETTING")]JObject order,
        ILogger log)
    {
        return new Person() {
                PartitionKey = "Orders",
                RowKey = Guid.NewGuid().ToString(),
                Name = order["Name"].ToString(),
                MobileNumber = order["MobileNumber"].ToString() };
    }
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
    public string MobileNumber { get; set; }
}
```

All the required parameters are passed in the class as annotations.

- [HTTP Triggers](https://learn.microsoft.com/en-us/azure/azure-functions/functions-bindings-http-webhook-trigger?tabs=python-v2%2Cisolated-process%2Cnodejs-v4%2Cfunctionsv2&pivots=programming-language-csharp)

## Azure Blob Storage

Three types of resources

- Storage Account - endpoint of the azure storage blob e.g mystorageaccount

`http://mystorageaccount.blob.core.windows.net`

- container -> organizes a set of blobs , a storage account can have unlimited containers

`https://myaccount.blob.core.windows.net/mycontainer`

- blobs
  1. block blobs - text and binary
  2. append blobs - logs
  3. page blobs - images for vm

`https://myaccount.blob.core.windows.net/mycontainer/myvirtualdirectory/myblob`

### Security

All files stored in azure storage are by default encrypted with 256 bit AES. A user can choose manual encryption by adding a key in microsoft valut

- customer managed key - A specific key is used to encrypt all the incoming data
- customer provided key - A key is provided in the request to read or write data in azure storage

### blob lifecycle policies

Any rule in a policy contains a filter set and a action set, filter set defines a set of data blobs and action set defines a particular action to be performed on the filtered set

### Rehydrate blob

Once a blob is in archive tier its considered as an offline resource, to make it alive , two options

- copy the contents of the blob to a new cool or hot tier blob
- set the existing blob to new tier

### Azure Storage Client

Azure storage provides classes for .NET to interact with the storage

| Class               | Description                                                                                                                                                    |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| BlobServiceClient   | Represents the storage account, and provides operations to retrieve and configure account properties, and to work with blob containers in the storage account. |
| BlobContainerClient | Represents a specific blob container, and provides operations to work with the container and the blobs within.                                                 |
| BlobClient          | Represents a specific blob, and provides general operations to work with the blob, including operations to upload, download, delete, and create snapshots.     |
| AppendBlobClient    | Represents an append blob, and provides operations specific to append blobs, such as appending log data.                                                       |
| BlockBlobClient     | Represents a block blob, and provides operations specific to block blobs, such as staging and then committing blocks of data.                                  |

- exercise to create blob storage using asp.net sdk client

/az-204/azblob/program.cs

1. create a new dotnet console app
2. add the azure sdk dependency
3. create a storage account , set up a container in it
4. use the BlobContainer class to update contents to it
5. clean up

### HTTP header for blobs

The standard HTTP headers supported on blobs include:

1. ETag
2. Last-Modified
3. Content-Length
4. Content-Type
5. Content-MD5
6. Content-Encoding
7. Content-Language
8. Cache-Control
9. Origin
10. Range

## Azure COSMOS DB

Azure cosmos DB is a NOSQL database which offers multi-master replication across all regions

hierarchy

DB account --> Databases --> container --> DB Items

![alt text](az2.png)

Cosmos DB supports stored procedures in interpretted languages (JS), all the procedures are time bound, during the creation of procedures a boolean return is expected indicating the status of the procedure

cosmos DB also offeres

- pre triggers --> these are initiated before making changes in the DB , used for formatting data, data validation
- post triggers --> these are initiated after making a change in DB , used for logging or sending an email

all the triggers are part of an single transaction, i.e if an trigger even fails the whole transaction is rolled back to initial state

A change feed is offered for a container in cosmos db , azure .net sdk or java sdk can be used to interact with it.

four steps in changed feed processor

1. connect to the container on which feed is required
2. A lease instance is issued to store the feed temporarily
3. A compute instance is initiated can be a app service or VM or physical
4. bussiness logic associated with change feed

## Azure Container Registry

Azure Container Registry (ACR) hosts all the services for a smooth CI/CD pipelines.

different tasks to build and maintain container images

- Quick task : build a single image and push to ACR
- Trigger Update: Source code trigger, Image update
- Multi updates: upgrades at container level , helm upgrades

### DockerFile

A Dockerfile is a script that contains a series of instructions that are used to build a Docker image. Dockerfiles typically include the following information:

- The base or parent image we use to create the new image
- Commands to update the base OS and install other software
- Build artifacts to include, such as a developed application
- Services to expose, such a storage and network configuration
- Command to run when the container is launched

## Azure Container Instances

It's a serveless container service in azure, main difference from the ACR is the ability to run container without managing the underlying images, useful for microservices , batch process and it provides various start and stop policies

Top level resource in ACI is `container-group` , it hosts multiple container under same host machine , all the containers share same life-cycle, resources and network connections

![alt text](az3.png)

This example container group:

- Is scheduled on a single host machine.
- Is assigned a DNS name label.
- Exposes a single public IP address, with one exposed port.
- Consists of two containers. One container listens on port 80, while the other listens on port 5000.
- Includes two Azure file shares as volume mounts, and each container mounts one of the shares locally.

multi container usecases

1. a container to host web application , a container to retrive and store logs
2. a container to host front-end , a container to run back-end services

### container restart policy

| Restart policy | Description                                                                                                                                                                                |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Always         | Containers in the container group are always restarted. This is the default setting applied when no restart policy is specified at container creation.                                     |
| Never          | Containers in the container group are never restarted. The containers run at most once.                                                                                                    |
| OnFailure      | Containers in the container group are restarted only when the process executed in the container fails (when it terminates with a nonzero exit code). The containers are run at least once. |

## Azure container Apps

This service runs on top of Azure Kubernetes service, mainly used for

- deploying api endpoints
- running background services
- running microservices
- handling event-driven services

All container Apps are serviced inside an Container Environment which shares the same virtual network resources and logging storage.

Azure container Apps cannot run priviliged apps , no root access is provided to containers and it can run only linux/amd64 based images

### Authorization and Authentication

Azure containers provide both authN and authZ services when `allowInsecure` is disabled and require authetication is enabled in the configuration settings.

In this Arch , both the authN and authZ run as sidecar containers running parallely.

![alt text](az4.png)

services provided by the middleware

- authN and authZ operations for selectede identity provider
- maintain session
- inject http headers with identity information

### revisions and secrets

- revisions : The version management is acheived through revisions, any specific container can have a current revision and can make updates to the current using an revision policy.

All the available revisions can be listed at

`az containerapp revision list`

- secrets : Secrets in container apps are declared at environment level, a change in secret doesn't creat or modify an exisiting revision. secrets are declared using the `--secret` variable during the init of the container and accessed using `secretref:`

### DAPR integration

Dapr --> Distributed Application Runtime

- it provides and acts as a medium to allow connections between containers to allow pub/sub messages and service to service call initiations
- Azure provides dapr and manages updates to it.

Services provided

1. service to service invocation - this allows discovery of the services andn service to service calls along with authN
2. state management - containers are stateless apps , this provides state for transactions and CRUD opxns
3. pub/sub - communication between containers through a middleware container
4. bindings - triggers to initiate services
5. actors - single thread workers which can be called or invoked through messages
6. observability - used to monitor containers
7. secrets - used to store secrets at dapr environment

![alt text](az5.png)

Dapr runs as side car containers in the container environment to provide services.

## Microsoft Identity Platform

Identify platform provides sdk and other options to let users sign in and use API's

allows

- microsoft entra accounts
- personal
- social

when regstering an application to Entra ID , need to mention whether its used by single tenant or group

- home tenant : home tenant of an application create an application object
- others: all other tenants of the application use service objects which are blueprints of the main application object class

### Authorization

- Oauth 2.0 : This service allows a third party app to use the application using mrst identity platform
- access to individual resources is provided by defining a scope, scopes are attached at the end of the resource URI
  e.g , accessing calender from the graph API
  `https://graph.microsoft.com/Calender.Read`
- graph --> service provider , Calender.Read --> scope
- permission types

1. delegate - user or app requests for access
2. app only - consent provided by admin for daemon processes

- consent types

1. static - provided by admin prior to accessing the app
2. dynamic - request access at run time
3. admin - high previlage access

### MSAL

the Microsoft Authorization Library offers services to authenticate users using Azure AD and provide access to secured web API's

Uses

- fetch tokens on behalf of application or an user
- handles a token cache repository , which auto issues tokens post ttl
- specify the audience to the application

Different types of authentication

| Flow               | Description                                                                   |
| ------------------ | ----------------------------------------------------------------------------- |
| Authorization code | Native and web apps securely obtain tokens in the name of the user            |
| Client credentials | Service applications run without user interaction                             |
| On-behalf-of       | The application calls a service/web API, which in turns calls Microsoft Graph |
| Implicit           | Used in browser-based applications                                            |
| Device code        | Enables sign-in to a device by using another device that has a browser        |
| Integrated Windows | Windows computers silently acquire an access token when they're domain joined |
| Interactive        | Mobile and desktops applications call Microsoft Graph in the name of a user   |
| Username/password  | The application signs in a user by using their username and password          |

Two different types of application clients are provided in azure

1. public client - the devices not trusted to store secrets and easy to access, they use web api's to fetch the secrets
2. confidential client - the devices which are trusted and hard to access , all the web api run on these servers.

with MSAL.NET , we can use `publicCLientApplicationBuilder`, `confidentialClientApplicationBuilder` methods to create these clients

Before the init process of the application , need to register the application in the azure identity, post that fetch the following from the portal

1. client id --> id of the application
2. identity provider URL
3. tenant ID --> used if the application group tenant application (organization)
4. appication secrets
5. redirectURI - the url to which user should be redirected along with tokens

#### Initiating Clients

- public client :

```c#
IPublicClientApplication app = PublicClientApplicationBuilder.Create(clientId).Build();
```

- confidentital client:

need to specify the redirect URI where the app server is running

```c#
string redirectUri = "https://myapp.azurewebsites.net";
IConfidentialClientApplication app = ConfidentialClientApplicationBuilder.Create(clientId)
    .WithClientSecret(clientSecret)
    .WithRedirectUri(redirectUri )
    .Build();
```

#### Access Modifiers

Access modifiers for public clients

| Modifier                                            | Description                                                                                                                                                                                                            |
| --------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| .WithAuthority()                                    | Sets the application default authority to a Microsoft Entra authority, with the possibility of choosing the Azure Cloud, the audience, the tenant (tenant ID or domain name), or providing directly the authority URI. |
| .WithTenantId(string tenantId)                      | Overrides the tenant ID, or the tenant description.                                                                                                                                                                    |
| .WithClientId(string)                               | Overrides the client ID.                                                                                                                                                                                               |
| .WithRedirectUri(string redirectUri)                | Overrides the default redirect URI. This is useful for scenarios requiring a broker.                                                                                                                                   |
| .WithComponent(string)                              | Sets the name of the library using MSAL.NET (for telemetry reasons).                                                                                                                                                   |
| .WithDebugLoggingCallback()                         | If called, the application calls Debug.Write simply enabling debugging traces.                                                                                                                                         |
| .WithLogging()                                      | If called, the application calls a callback with debugging traces.                                                                                                                                                     |
| .WithTelemetry(TelemetryCallback telemetryCallback) | Sets the delegate used to send telemetry.                                                                                                                                                                              |

Access modifiers for confidential clients

| Modifier                                       | Description                                                                                |
| ---------------------------------------------- | ------------------------------------------------------------------------------------------ |
| .WithCertificate(X509Certificate2 certificate) | Sets the certificate identifying the application with Microsoft Entra ID.                  |
| .WithClientSecret(string clientSecret)         | Sets the client secret (app password) identifying the application with Microsoft Entra ID. |

## Shared Access

A shared access signature (SAS) is a URI that grants restricted access rights to Azure Storage resources. You can provide a shared access signature to clients that you want to grant delegate access to certain storage account resources.

Three types of shared access

1. User SAS - SAS secured with Entra ID credentials , used for blob storage
2. Service SAS - secured with storage account key , used to access blob storage, queue storage, files
3. Account SAS - used to delegate access to resources dependent on storage

When you use a SAS to access data stored in Azure Storage, you need two components. The first is a URI to the resource you want to access. The second part is a SAS token that you've created to authorize access to that resource.
In a single URI, such as ``https://medicalrecords.blob.core.windows.net/patient-images/patient-116139-nq8z7f.jpg?sp=r&st=2020-01-20T11:42:32Z&se=2020-01-20T19:42:32Z&spr=https&sv=2019-02-02&sr=b&sig=SrW1HZ5Nb6MbRzTbXCaPm%2BJiSEn15tC91Y4umMPwVZs%3D``,

you can separate the URI from the SAS token as follows:
URI: `https://medicalrecords.blob.core.windows.net/patient-images/patient-116139-nq8z7f.jpg?`
SAS token: `sp=r&st=2020-01-20T11:42:32Z&se=2020-01-20T19:42:32Z&spr=https&sv=2019-02-02&sr=b&sig=SrW1HZ5Nb6MbRzTbXCaPm%2BJiSEn15tC91Y4umMPwVZs%3D`

| Component                                            | Description                                                                                                                                                                                                    |
| ---------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| sp=r                                                 | Controls the access rights. The values can be a for add, c for create, d for delete, l for list, r for read, or w for write. This example is read only. The example sp=acdlrw grants all the available rights. |
| st=2020-01-20T11:42:32Z                              | The date and time when access starts.                                                                                                                                                                          |
| se=2020-01-20T19:42:32Z                              | The date and time when access ends. This example grants eight hours of access.                                                                                                                                 |
| sv=2019-02-02                                        | The version of the storage API to use.                                                                                                                                                                         |
| sr=b                                                 | The kind of storage being accessed. In this example, b is for blob.                                                                                                                                            |
| sig=SrW1HZ5Nb6MbRzTbXCaPm%2BJiSEn15tC91Y4umMPwVZs%3D | The cryptographic signature.                                                                                                                                                                                   |

---

![alt text](az6.png)

Use case

- A client wants to read/write information to a storage account of another user/owner.
- In order to facilitate this an SAS is required
- A front end proxy server is established to validate the files if required and get an SAS
- Post acquring the SAS , the files are sent to the storage account

An extra level of security can be acheived by adding the storage access policy to a container to check on the server side before accessing the file

- creating a storage access policy throuh azure-cli

```bash
az storage container policy create --name <stored access policy identifier> --container-name <container name> --start <start time UTC datetime> --expiry <expiry time UTC datetime> --permissions <(a)dd, (c)reate, (d)elete, (l)ist, (r)ead, or (w)rite> --account-key <storage account key> --account-name <storage account name>
```
