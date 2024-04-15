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

```
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

```
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
