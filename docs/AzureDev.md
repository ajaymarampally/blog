# Azure Developer Associate

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

### how it wokrs

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

