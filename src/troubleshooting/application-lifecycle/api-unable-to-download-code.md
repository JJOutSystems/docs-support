---
summary: If you are unable to download the source code, you can troubleshoot by validating factors, such as full-control, application key, environment key, etc. of an application.
tags: 
locale: en-us
guid: E542C662-6053-4758-A514-1A4117364DD0
app_type: traditional web apps, mobile apps, reactive web apps
platform-version: o11
---

# Unable to download the source code of an application

If you are getting an error downloading the source code package of an application from the steps mentioned in [this article](https://success.outsystems.com/documentation/11/reference/outsystems_apis/lifetime_api_v2/lifetime_api_examples/get_source_code_of_an_application/), you can troubleshoot a potential issue by following the steps below. 

## 1 - Check if the user has full control over the application

To validate this requirement, follow these steps:

1. In the LifeTime management console, open the **User Management** tab and select the Service Accounts sub-menu.
1. Select the service account used on the source code access API by clicking the service account name.
1. Check if the level of permission of the user is Full-Control on the environment you are requesting the source code.

The wrong permissions level returns a message: `You need permission to access the environment {EnvironmentKey} or the app {ApplicationKey}.`

## 2 - Check if the environment has Non-Production purpose

Call the API method that returns the details of the environment.
Request: `GET /environments/{EnvironmentKey}/`

Response body:

```
{
  "Key": "849515f2-b4ff-4aca-a9d6-9407bea655f4",
  "Name": "Testing",
  "OSVersion": "11.0.110.0",
  "Order": 1,
  "HostName": "prd-env-1.company.com",
  "UseHTTPS": true,
  "EnvironmentType": "NonProduction",
  "NumberOfFrontEnds": 1,
  "ApplicationServerType": ".NET",
  "ApplicationServer": "IIS",
  "DatabaseProvider": "SQLServer",
  "IsCloudEnvironment": false
}
```
Confirm that the environment on which you are requesting the source code has the `EnvironmentType` as `NonProduction`.
A request to the wrong purpose environment returns the message: `Couldn't request the app source code. You can only do it in a non-production environment.`


## 3 - Check if the environment key exists

Call the API method that returns all the environments registered on your infrastructure.
Request: `GET /lifetimeapi/rest/v2/environments/`

Response body:

```
[
  {...},
  {
    "Key": "849515f2-b4ff-4aca-a9d6-9407bea655f4",
    "Name": "Testing",
    "OSVersion": "11.0.110.0",
    "Order": 1,
    "HostName": "prd-env-1.company.com",
    "UseHTTPS": true,
    "EnvironmentType": "Production",
    "NumberOfFrontEnds": 1,
    "ApplicationServerType": ".NET",
    "ApplicationServer": "IIS",
    "DatabaseProvider": "SQLServer",
    "IsCloudEnvironment": false
  },
  {...}
]
```

Check that the environment key (**Key**) is correctly passed to the API call.

If the environment key does not exist the API will return a message: `The environment {EnvironmentKey} or the app {ApplicationKey} doesn't exist.`


## 4 - Check if the application key exists

Call the API method that returns all the applications available on your infrastructure.

Request: `GET /lifetimeapi/rest/v2/applications/`

Response body:

```
[
  {...},
  {
    "Key": "c9a7a82e-0eee-4a3d-8e22-2a19c69c766f",
    "Name": "EmployeeBackoffice",
    "Kind": "WebResponsive",
    "Team": "",
    "Description": "",
    "URLPath": "/EmployeeBackoffice",
    "IconHash": "IconHash6a79e71e-c8e5-9e18-115c-cab789517672",
    "IconURL": "/LifeTimeSDK/ApplicationIcon.aspx?ApplicationKey=c9a7a82e-0eee-4a3d-8e22-2a19c69c766f",
    "IsSystem": false,
    "AppStatusInEnvs": []
  },
  {...}
]

```

Check that the application key (**Key**) is correctly passed to the API call.
If the application key does not exist the API will return a message: `The environment {EnvironmentKey} or the app {ApplicationKey} doesn't exist.`


If the above validations didn't help you solve the issue and you need further assistance, [open a support case](https://www.outsystems.com/SupportPortal/CaseOpen/) to get help from OutSystems Support.
