### Links

[Download scripts from GitHub](https://github.com/Oyon84/AD-Tier-Administration/archive/refs/heads/master.zip)

[GitHub Repo](https://github.com/Oyon84/AD-Tier-Administration)

# Powershell scripts for AD Tiering model deployment

**This set of PowerShell scripts provides a streamlined way to deploy a tiered model in Active Directory. It’s designed for both existing AD environments—where it creates a new top-level OU and implements the tiering model—and brand-new AD setups, allowing you to establish a tiered structure right from the start.**

It’s recommended to first test the deployment in a lab environment. This will help you become familiar with the scripts and associated files, ensuring a smooth and confident rollout in production.

## Requirements
- AD domain
- Domain admin level permissions
- Text editor to edit the input files (csv / json / log)
- Powershell client, can be visual studio code or Powershell ISE or other preference

## Steps
1. Deploy OU / Container Structure (Create-Structure.ps1)
2. Creating groups (Create-Groups.ps1)
3. Creating Password Settings Objects (Create-PSOs.ps1)
4. Creating Group Policy Objects (Create-GPOs.ps1)
5. Creating AD roles and permissions (Create-ACEs.ps1)

# 1 - Deploy OU / Container Structure (Create-Structure.ps1)
First step is to create a new OU structure. This script will deploy a new Top Level OU called **corp** and deploys a new OU structure in the corp OU. This will contain OU's for all tiers and assets and this forms the basis of the tiering model. This script also deploys a set of containers for the following assets:
- Roles Tier 0, 1, 2
- Access Control Tier 0, 1, 2
- Security Groups

The script will perform a check the object after its created, if something failed it will be shown on the console with Red text displaying the error.

## Prerequisites 
- Rename user locations in **Create-Structure.ps1** to something sensible for your organization. Currently marked as Location 1, 2, 3, 4 and 5.
- Rename path to your domain. Current value is "DC=test,DC=local" standing for test.local as domain name. Use your text editor to find DC=test,DC=local and replace with DC=yourdomain,DC=local
- Create any extra OU's 

## Run script
Next step is to run the script. Output should look similair to the following: 

```
PS C:\Users\Administrator> C:\Temp\Create-Structure.ps1
Creating OU Corp in DC=test,DC=local
Creating OU Users in OU=Corp,DC=test,DC=local
Creating OU Service in OU=Corp,DC=test,DC=local
Creating OU Administration in OU=Corp,DC=test,DC=local
Creating OU Computers in OU=Corp,DC=test,DC=local
Creating OU Tier 2 in OU=Computers,OU=Corp,DC=test,DC=local
Creating OU Tier 1 in OU=Computers,OU=Corp,DC=test,DC=local
Creating OU Tier 0 in OU=Computers,OU=Corp,DC=test,DC=local
Creating OU Terminal servers in OU=Computers,OU=Corp,DC=test,DC=local
Creating OU PKI in OU=Tier 0,OU=Computers,OU=Corp,DC=test,DC=local
Creating OU Shared in OU=Tier 2,OU=Computers,OU=Corp,DC=test,DC=local
Creating OU Desktops in OU=Tier 2,OU=Computers,OU=Corp,DC=test,DC=local
Creating OU Laptops in OU=Tier 2,OU=Computers,OU=Corp,DC=test,DC=local
Creating OU Network in OU=Tier 1,OU=Computers,OU=Corp,DC=test,DC=local
Creating OU Application in OU=Tier 1,OU=Computers,OU=Corp,DC=test,DC=local
Creating OU File in OU=Tier 1,OU=Computers,OU=Corp,DC=test,DC=local
Creating OU Update in OU=Tier 1,OU=Computers,OU=Corp,DC=test,DC=local
Creating OU Delft in OU=Users,OU=Corp,DC=test,DC=local
Creating OU Location2 in OU=Users,OU=Corp,DC=test,DC=local
Creating OU Location3 in OU=Users,OU=Corp,DC=test,DC=local
Creating OU Location4 in OU=Users,OU=Corp,DC=test,DC=local
Creating OU Location5 in OU=Users,OU=Corp,DC=test,DC=local
Creating OU Offsite in OU=Users,OU=Corp,DC=test,DC=local
Creating OU Contractors in OU=Users,OU=Corp,DC=test,DC=local
Creating OU Tier 0 in OU=Service,OU=Corp,DC=test,DC=local
Creating OU Tier 1 in OU=Service,OU=Corp,DC=test,DC=local
Creating OU Users in OU=Tier 0,OU=Service,OU=Corp,DC=test,DC=local
Creating OU Users in OU=Tier 1,OU=Service,OU=Corp,DC=test,DC=local
Creating OU Tier 0 in OU=Administration,OU=Corp,DC=test,DC=local
Creating OU Tier 1 in OU=Administration,OU=Corp,DC=test,DC=local
Creating OU Tier 2 in OU=Administration,OU=Corp,DC=test,DC=local
Creating OU Users in OU=Tier 0,OU=Administration,OU=Corp,DC=test,DC=local
Creating OU Users in OU=Tier 1,OU=Administration,OU=Corp,DC=test,DC=local
Creating OU Users in OU=Tier 2,OU=Administration,OU=Corp,DC=test,DC=local
Creating Container Security groups in OU=Corp,DC=test,DC=local
Creating Container Access Control in CN=Security groups,OU=Corp,DC=test,DC=local
Creating Container Roles in CN=Security groups,OU=Corp,DC=test,DC=local
Creating Container Access Control in OU=Tier 0,OU=Administration,OU=Corp,DC=test,DC=local
Creating Container Roles in OU=Tier 0,OU=Administration,OU=Corp,DC=test,DC=local
Creating Container Access Control in OU=Tier 1,OU=Administration,OU=Corp,DC=test,DC=local
Creating Container Roles in OU=Tier 1,OU=Administration,OU=Corp,DC=test,DC=local
Creating Container Access Control in OU=Tier 2,OU=Administration,OU=Corp,DC=test,DC=local
Creating Container Roles in OU=Tier 2,OU=Administration,OU=Corp,DC=test,DC=local
Creating Container Access Control in OU=Tier 1,OU=Service,OU=Corp,DC=test,DC=local
Creating Container Access Control in OU=Tier 0,OU=Service,OU=Corp,DC=test,DC=local

PS C:\Users\Administrator> 
```

Fix any error you come accross. And the Structure deployment is done.

# 2 - Creating groups (Create-Groups.ps1)
Next step is to create groups that will be used for different purposes. The "**group-creation.csv**" file contains all default groups needed for a basic tiering model. You can add extra groups as you desire in the same format.

## Prerequisites 
- Again you will have to correct the domain name, but this time in the CSV file. Open the CSV file in a text editor and replace the same values as in step 1 to match you domain.
- Store the script file (Create-Groups.ps1) and the CSV file (group-creation.csv) in the same folder. The script looks for the CSV file in the root where the script is executed. If you use ISE, go to the location of the script, for example: ```cd c:\temp```
- Add any custom groups you want to deploy as well to the CSV file, see example:
```
Name,Path,GroupScope,GroupCategory
COMPTR_Default Container_CREATE,"CN=Access Control,OU=Tier 2,OU=Administration,OU=Corp,DC=test,DC=local",DomainLocal,Security
```

## Run Script
Now you can run the script, the output should give you information about the groups successfully created and als the ones that failed to be created. Check for errors in the output, correct them as needed and run the script again. Groups that already exist will be skipped, the ones that failed will be created if the issue is resolved.

```
If the script gives an error that it can't find the CSV file check if the script and csv are in the same folder
and your console location is that folder.
```
Once all groups are greated and the script only give "skipping" messages you can continue with the next step.

# 3 - Creating Password Settings Objects (Create-PSOs.ps1)
PSO's are objects that enforce password requirements. Using PSO's gives the ability to set different scopes of password requirements per tier. This script does not require an input file.

## Prerequisites
- Check the script file to validate the password settings are according to your organizations need. There are 5 objects defined in the script.

  | # | Scope                  | Object                | Applies to                     | Max Age | Min Lenght | History Count | Precedence |
  |---|------------------------|-----------------------|--------------------------------|---------|------------|---------------|------------|
  | 1 | Tier 0 Admin           | PSO_AT0_Administrator | PSO_Tier 0 ADM Users_APPLY     | 120     | 20         | 20            | 10         |
  | 2 | Tier 1 Admin           | PSO_AT1_Administrator | PSO_Tier 1 ADM Users_APPLY     | 120     | 20         | 20            | 20         |
  | 3 | Tier 2 Admin           | PSO_AT2_Administrator | PSO_Tier 2 ADM Users_APPLY     | 120     | 16         | 20            | 30         |
  | 4 | Tier 0 Service Account | PSO_ST0_Service User  | PSO_Tier 0 Service Users_APPLY | 10000   | 100        | 20            | 10         |
  | 5 | Tier 1 Service Account | PSO_ST1_Service User  | PSO_Tier 1 Service Users_APPLY | 10000   | 100        | 20            | 20         |
  
  ```Note: for all PSO's complexity is turned on requiring complex passwords```
- Validate the settings with your organizations security policies, if you don't have any the default settings are a good starting point.

## Run Script
When all settings are correct you can run the script. This will create the Password Settings Objects and link them to the correct groups. This will enforce the password policies to the right accounts when all scripts are deployed.

```
**Note:**This script has no output, so if the script finished without any output it has run successful.
To verify if objects are created you can run the script again, and it should tell you the objects already exist.
```
# 4 - Creating Group Policy Objects (Create-GPOs.ps1)
The following script does 3 things:
- Create GPO according input given
- Set permission based on admin level
- Link GPO to correct OU
What this script does not do is setting the settings for these GPO.

## Naming Convention
This script checkes for naming compliance and uses the following rules
- Should start with C | U | UC | CU
- Policy type in capitals, for example: SEC, ADM, PRINTER, Max 15 chars
- Policy name

During execution of the script with both methods the naming compliance is checked and if fails the scripts breaks.

## Admin Levels
The script defines the following admin levels. These translate to groups created prior and grant permissions on GPO's. Below an overview of the default admin levels and linked groups.
```
Tier0Global                       = "GPO_Tier 0 Global GPO_MOD"
Tier0PKIServers                   = "GPO_Tier 0 PKI SRV_MOD"
Tier0PrivilegedAccessWorkstations = "GPO_Tier 0 Privileged Access Workstations_MOD"
Tier1AllServers                   = "GPO_Tier 1 All SRV_MOD"
Tier1ADMUsers                     = "GPO_Tier 1 ADM Users_MOD"
Tier1ServiceUsers                 = "GPO_Tier 1 Service_MOD"
Tier1NetServers                   = "GPO_Tier 1 Network SRV_MOD"
Tier1AppServers                   = "GPO_Tier 1 Application SRV_MOD"
Tier1FileServers                  = "GPO_Tier 1 File SRV_MOD"
Tier1UpdateServers                = "GPO_Tier 1 Update SRV_MOD"
Tier1TermServers                  = "GPO_Tier 1 Terminal SRV_MOD"
Tier1PrivilegedAccessWorkstations = "GPO_Tier 1 Privileged Access Workstations_MOD"
Tier2Workstations                 = "GPO_Tier 2 Workstations_MOD"
CorporateUsers                    = "GPO_Corporate Users_MOD"
```
If you have changed the group names in step 2, that should also be changed in this step. 

## Run Script
The script has two excution methods.
- File (Use CSV input file)
- Interactive (Use console to enter GPO name and admin level)

For initial deployment its reconmended to use the File input method, check the CSV file and adjust to requirements for the organization.
```
PS C:\Temp> .\Create-GPOs.ps1 C:\Temp\gpo-list.csv
```
If the script has run successful it should show the following output for each GPO in the CSV file.
```
Creating GPO C_PWR_Desktop power management
DisplayName      : C_PWR_Desktop power management
DomainName       : test.local
Owner            : TEST\Domain Admins
Id               : cdb51cfc-7158-4829-8a59-03b10b7e5320
GpoStatus        : AllSettingsEnabled
Description      : 
CreationTime     : 11/14/2024 12:17:04 PM
ModificationTime : 11/14/2024 12:17:04 PM
UserVersion      : AD Version: 0, SysVol Version: 0
ComputerVersion  : AD Version: 0, SysVol Version: 0
WmiFilter        : 


Setting permission to GPO C_PWR_Desktop power management
DisplayName      : C_PWR_Desktop power management
DomainName       : test.local
Owner            : TEST\Domain Admins
Id               : cdb51cfc-7158-4829-8a59-03b10b7e5320
GpoStatus        : AllSettingsEnabled
Description      : 
CreationTime     : 11/14/2024 12:17:04 PM
ModificationTime : 11/14/2024 12:17:04 PM
UserVersion      : AD Version: 0, SysVol Version: 0
ComputerVersion  : AD Version: 0, SysVol Version: 0
WmiFilter        : 


Linking GPO C_PWR_Desktop power management to OU OU=Desktops,OU=Tier 2,OU=Computers,OU=Corp,DC=test,DC=local

DisplayName   : C_PWR_Desktop power management
GpoId         : cdb51cfc-7158-4829-8a59-03b10b7e5320
Enabled       : True
Enforced      : False
Order         : 1
Target        : OU=Desktops,OU=Tier 2,OU=Computers,OU=Corp,DC=test,DC=local
GpoDomainName : test.local
```
All GPO objects are deployed and linked to the correct OU. Next step is to configure settings for the GPO's, but that part is not covered in this guid.

# 5 - Creating AD roles and permissions (Create-ACEs.ps1)
This script deploys permissions to groups that can be used in roles. 
