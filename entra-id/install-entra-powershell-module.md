# Index

- [Index](#index)
- [Introduction](#introduction)
    - [Requirements](#requirements)
- [Install Module](#install-module)
    - [Verify Installation](#verify-installation)
- [Sign in](#sign-in)
    - [Delegated Access](#delegated-access)
    - [App Only Access](#app-only-access)
    - [Scopes](#scopes)
- [Manage Users](#manage-users)
    - [Get an Entra User](#get-an-entra-user)
    - [Create a new User](#create-a-new-user)
    - [Conclusion](#conclusion)

# Introduction

Install the powershell module for Entra ID. This allows you to manage Entra ID with Powershell. The Entra cmdlets make use of Microsoft Graph, a API thats allows to access many types of data from Microsoft 365, Entra ID, Intune and other services. 

### Requirements

The following requirements need to be met.
- Powershell 7.X or 5.1.X
- Entra ID Tenant
- User with enough permissions

If you use Powershell 5 you need the following requirements as well:
- .NET Framework 4.7.2 or later
- PowerShellGet

To install or update PowerShellGet run the following command:
```
Install-Module -Name PowerShellGet -Force -AllowClobber
```

# Install Module

Use the Install-Module cmdlet to install the module.

```
Install-Module -Name Microsoft.Graph.Entra -Repository PSGallery -Scope CurrentUser -AllowPrerelease -Force
```

If you want to install the module for all users:
```
Install-Module -Name Microsoft.Graph.Entra -Repository PSGallery -Scope AllUsers -AllowPrerelease
```

To install the Beta version:
```
Install-Module -Name Microsoft.Graph.Entra.Beta -Repository PSGallery -AllowPrerelease
```

> In Powershell 5 you can get an error: *"Function {cmdlet-name} cannot be created because function capacity 4096 has been exceeded."* Run the following command: **$MaximumFunctionCount = 32768**  

### Verify Installation

To check if the installation was successful run the following command:
```
PS C:\Temp> Get-InstalledModule -Name Microsoft.Graph.Entra

Version              Name                                Repository           Description
-------              ----                                ----------           -----------
0.19.0-preview       Microsoft.Graph.Entra               PSGallery            Microsoft Graph Entra PowerShell.
```

```
PS C:\Temp> Get-InstalledModule

...
2.15.0               Microsoft.Graph.Applications        PSGallery            Microsoft Graph PowerShell Cmdlets
2.15.0               Microsoft.Graph.Authentication      PSGallery            Microsoft Graph PowerShell Authentication Module.
0.19.0-preview       Microsoft.Graph.Entra               PSGallery            Microsoft Graph Entra PowerShell.
2.15.0               Microsoft.Graph.Groups              PSGallery            Microsoft Graph PowerShell Cmdlets
2.15.0               Microsoft.Graph.Identity.Directory… PSGallery            Microsoft Graph PowerShell Cmdlets
2.15.0               Microsoft.Graph.Identity.Governance PSGallery            Microsoft Graph PowerShell Cmdlets
2.15.0               Microsoft.Graph.Identity.SignIns    PSGallery            Microsoft Graph PowerShell Cmdlets
6.1907.1.0           Microsoft.Graph.Intune              PSGallery            PowerShell SDK for Microsoft Intune Graph API
2.15.0               Microsoft.Graph.Reports             PSGallery            Microsoft Graph PowerShell Cmdlets
2.15.0               Microsoft.Graph.Users               PSGallery            Microsoft Graph PowerShell Cmdlets
2.15.0               Microsoft.Graph.Users.Actions       PSGallery            Microsoft Graph PowerShell Cmdlets
2.15.0               Microsoft.Graph.Users.Functions     PSGallery            Microsoft Graph PowerShell Cmdlets
```

# Sign in

### Delegated Access

Delegated access means you are using the permissions of the logged in user. To sign in to Entra ID you run the following command:

```
PS C:\Temp> Connect-Entra -Scopes 'User.Read.All'
Welcome to Microsoft Graph!

Connected via delegated access using xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
Readme: https://aka.ms/graph/sdk/powershell
SDK Docs: https://aka.ms/graph/sdk/powershell/docs
API Docs: https://aka.ms/graph/docs

NOTE: You can use the -NoWelcome parameter to suppress this message.
```

When you do not have an active session you will be prompted with a Microsoft Login, once you enter your email address you will be redirected to your tenants login screen and you can complete the login, close the browser and start using *Microsoft Graph*.  

### App Only Access

App only access uses app credentials to setup a session. You can use client secrets or certificates. To login with client secret use the following code as an example:

```
$ClientSecretCredential = Get-Credential -Credential '00001111-aaaa-2222-bbbb-3333cccc4444'
# Enter client_secret in the password prompt.
Connect-Entra -TenantId 'aaaabbbb-0000-cccc-1111-dddd2222eeee' -ClientSecretCredential $ClientSecretCredential
```

### Scopes

For and app or user to access data through the Microsoft Graph API you need to define a scope. In the example above we used the scope *'User.Read.All'* which allows the user or app to read the full profile of all users in your organization. If you would want to make changes to users profile you would have to use the scope *'User.ReadWrite.All'*.  

> There are many more scopes, for a full overview see [Graph Permissions Reference](https://learn.microsoft.com/en-us/graph/permissions-reference)  

# Manage Users

Now that we are signed in we can perform some action.

### Get an Entra User

To get an user from Entra ID run the following command:
```
PS C:\Temp> Get-entraUser -Filter "userPrincipalName eq 'test@fhs7.nl'"

DisplayName Id                                   Mail         UserPrincipalName
----------- --                                   ----         -----------------
test user   XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX test@fhs7.nl test@fhs7.nl
```

### Create a new User

To create a new user you need to login with the *'User.ReadWrite.All'* scope and run the following commands.

```
PS C:\Temp> connect-entra -Scopes 'User.ReadWrite.All' -NoWelcome
PS C:\Temp> $passwordProfile = New-Object -TypeName Microsoft.Open.AzureAD.Model.PasswordProfile
PS C:\Temp> $passwordProfile.Password = '$tr0ngP@ssw0rd'
PS C:\Temp> $userParams = @{
>> DisplayName = 'Test2 User'
>> PasswordProfile = $passwordProfile
>> UserPrincipalName = 'test2@fhs7.nl'
>> AccountEnabled = $true
>> MailNickName = 'test2user'
>> }
PS C:\Temp> New-EntraUser @userParams

DisplayName Id                                   Mail UserPrincipalName
----------- --                                   ---- -----------------
Test2 User  45304f10-254a-4aec-ab51-b6281304105e      test2@fhs7.nl
```

The *userParams* var can be extended with as many user properties as you want. 

### Conclusion

The Entra ID powershell module is a powerfull tool as part of the Graph API. You can either use it in user context or in application context. There is far more possible than what we did here and there are other Microsoft Graph modules as well offering even more functionality. 

To learn more about the Microsoft Graph Entra Powershell module you can visit [Microsoft Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.graph.entra/?view=entra-powershell).