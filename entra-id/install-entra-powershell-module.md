# Index
- [Index](#index)
# Introduction

Install the powershell module for Entra ID. This allows you to manage Entra ID with Powershell.

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

# Verify Installation

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

To sign in to Entra ID you run the following command:

```
PS C:\Temp> Connect-Entra -Scopes 'User.ReadWrite.All'
Welcome to Microsoft Graph!

Connected via delegated access using xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
Readme: https://aka.ms/graph/sdk/powershell
SDK Docs: https://aka.ms/graph/sdk/powershell/docs
API Docs: https://aka.ms/graph/docs

NOTE: You can use the -NoWelcome parameter to suppress this message.
```

### Scopes

With scopes you can set 