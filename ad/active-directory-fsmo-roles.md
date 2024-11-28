# Index

- [Index](#index)
- [Introduction](#introduction)
    - [Purpose of FSMO roles](#purpose-of-fsmo-roles)
- [FSMO Roles](#fsmo-roles)
    - [Schema Master Role](#schema-master-role)
    - [Domain Naming Master Role](#domain-naming-master-role)
    - [RID Master Role](#rid-master-role)
    - [PDC Emulator role](#pdc-emulator-role)
    - [Infrastructure Master role](#infrastructure-master-role)
- [Manage FSMO Roles](#manage-fsmo-roles)
    - [Move Role from one to another DC](#move-role-from-one-to-another-dc)
    - [View FSMO roles with GUI](#view-fsmo-roles-with-gui)
    - [View FSMO roles with Powershell](#view-fsmo-roles-with-powershell)
    - [View FSMO roles with Command Prompt](#view-fsmo-roles-with-command-prompt)

# Introduction

This article describes the FSMO roles within Active Directory and how to identify the Domain Controllers hosting these roles. 

Active Directory defines five FSMO roles:
- Schema Master
- Domain Naming Master
- Relative ID (RID) Master
- Primary Domain Controller (PDC) Emulator
- Infrastructure Master (domain level)

> The same DC can be assigned multiple FSMO roles, or even all five of them. The DC that is assigned a particular FSMO role is called the role owner.

### Purpose of FSMO roles
We need to go back in time to understand the purpose of AD FSMO roles. Early versions of Windows used a **single-master** model. One DC, the primary domain controller (PDC), would be responsible for processing all updates in a particular domain. This model makes *conflicts* impossible, there is a major drawback to this approach. When the PDC was unavailable its impossible to process updates.

Going back to today, Windows uses a **multi-master** model. Changes to AD can be processed by any domain controller and are then replicated to the other DCs. While this model eliminates the problem of having a single point of failure, it leads to a different issue: What if two or more DCs process conflicting updates?

Conflicts are resolved using a simple algorithm, *the last writer wins*. Any changes made before will be discarded. In most cases this works fine, but some changes are to important to depend on this simple algorithm. For those changes only one DC is permitted to process these changes, the FSMO role owner DC. 

![FSMO Roles](https://www.quest.com/images/patterns/text-image/fsmo-roles.png)  

# FSMO Roles

This section goes over all the five FSMO roles.

### Schema Master Role

This DC is responsible for processing updates to the directory schema, which are then replicated to all other DCs in the directory. The schema is the blueprint of an Active Directory, with formal definitions of every object class (such as user, computer and security group) that can be created in the AD forest, and every attribute that can exist in an AD object.  

> There can be only one Schema Master in a forest, no mater how many domains the forest contains. 

For example, when you want to add an attribute to the User Object, this would be a change in the Schema. Some basic attributes for an User Object are:
- First Name
- Last Name
- Email Address
- Password

Some applications (such as Exchange) require extra attributes to be added to Active Directory Objects in order to function properly. 

### Domain Naming Master Role

This DC is responsible for adding and removing domains in the forest. In particular, it ensures that each domain in the forest has a unique name. The Domain Naming Master can also add or remove cross references to domains in external directories.

> In a given forest, only one DC has the Domain Naming Master role.

### RID Master Role

This DC is responsible for processing RID pool requests from all DCs in the domain. Each user, group and other security principal is assigned a unique security identifier (SID) when it is created, one component of which is a unique RID. Therefore, each DC needs a pool of RIDs it can assign to the objects it creates. When it runs low, it requests another RID pool from the RID Master.

> In each domain, one domain controller is assigned the RID Master role.

The RID Master is also responsible for removing an object from its domain and putting it in another domain during an object move.

### PDC Emulator role

This DC is responsible for synchronizing time, which is vital for the integrity of many processes, including Kerberos authentication. The PDC Emulator of a domain is authoritative for that domain. The PDC Emulator for the domain at the root of the forest is authoritative for the enterprise; it should be configured to collect the time from an external source. 

> In each domain, one DC is assigned the PDC Emulator role. The PDC Emulator for the domain at the root of the forest is authoritative for the enterprise.

The PDC Emulator is particularly important for password changes and failed authentication events:
- Password changes processed by other DCs in the domain are replicated preferentially to the PDC Emulator.
- When an authentication attempt fails due to an incorrect password, the failure is sent to the PDC Emulator before the failure message is issued to the user.
- Account lockouts are processed on the PDC Emulator.

### Infrastructure Master role

This DC is responsible for updating an object's SID and distinguished name in a cross-domain object reference. In particular, it makes sure that names, rather than cryptic SIDs, appear in your access control lists (ACLs).

> In each domain, one DC is assigned the Infrastructure Master role.

This role should be assigned to a DC that is not a Global Catalog (GC) server, unless all the DCs in a domain host the global catalog; in that case, it does not matter which one has the Infrastructure Master role since all the DCs will have current data.

# Manage FSMO Roles

This section dives into managing the FSMO roles, there are some do and don't recommendations.

### Move Role from one to another DC

The DC that owns an FSMO role needs to be available whenever updates it is responsible for need to be made. Some of those updates, such as change to the schema and the addition of new domains, are rare, while other types of changes are more frequent. In particular, the PDC Emulator needs to be accessible at all times.  

Administrators can move an Active Directory FSMO role from one DC to another via transfer or seizure:
- **Transfer** - One reason to transfer a role to another DC is that you know that a particular FSMO role owner is going be offline when it will be needed. When the former role holder restarts, it will check inbound replication information, determine that it no longer holds the role and smoothly relinquish role ownership.
- **Seizure** -  If a DC that owns an FSMO role crashes and cannot be restored, you can seize the FSMO role and assign it to another domain controller.

> When performing a transfer, to complete the procedure the current role owner needs to reboot to release ownership of the role.

To use powershell to move the roles to other DC's you need to keep the following numbers in mind:
```
0: PDCEmulator
1: RIDMaster
2: InfrastructureMaster
3: SchemaMaster
4: DomainNamingMaster
```
These numbers can be used to identify the role you want to move, see commands below.  
<sub>*Transfer roles*</sub>
```
Move-ADDirectoryServerOperationMasterRole -Identity NewDC -OperationMasterRole 0,1,2,3,4  
```
<sub>*Seize roles*</sub>
```
Move-ADDirectoryServerOperationMasterRole -Identity NewDC -OperationMasterRole 0,1,2,3,4 -Force  
```

### View FSMO roles with GUI

The following procedures allow you to view the ownership of the FSMO roles. If you want to use the GUI there are 3 different places to collect all roles. If you use command prompt or powershell you can get all role owners in one go.

**RID Master, PDC Master or Infrastructure Master role**  

To determine which DC holds the RID Master, PDC Master or Infrastructure Master role in a particular domain, take the following steps:
- Run the command dsa.msc
- Right-click the selected domain object in the top left pane, and then click Operations Masters
- Click the appropriate tab: RID Pool, PDC or Infrastructure

**Schema Master role**  

To determine which DC holds the Schema Master role in the forest:
- Run the command dsc.msc
- On the Console menu, click Add/Remove Snap-in, click Add, double-click Active Directory Schema, click Close, and then click OK
- In the top-left pane, right-click Active Directory Schema, and then click Operations Masters

**Domain Naming Master role**  

To determine which DC holds the Domain Naming Master role in the forest:
- Run the command dsa.msc
- On the Console menu, click Add/Remove Snap-in, click Add, double-click Active Directory Domains and Trusts, click Close, and then click OK
- In the left pane, click Active Directory Domains and Trusts
- Right-click Active Directory Domains and Trust, and then click Operations Master

### View FSMO roles with Powershell

Using powershell I have created a small script that will output a table with all role owners.


<sub>*Get domain FSMO roles*</sub>
```
PS C:\Temp> Get-ADDomain test.local | format-table PDCEmulator,RIDMaster,InfrastructureMaster

PDCEmulator                RIDMaster                  InfrastructureMaster      
-----------                ---------                  --------------------      
DC01.test.local            DC01.test.local            DC01.test.local
```

<sub>*Get forest FSMO roles*</sub>
```
PS C:\Temp> Get-ADForest test.local | Format-Table SchemaMaster,DomainNamingMaster

SchemaMaster               DomainNamingMaster        
------------               ------------------        
DC01.test.local            DC01.test.local
```

These two commands give you all the role owners within your Active Directory Domain or Forest. Is there an even faster way?

### View FSMO roles with Command Prompt

To get all the role owners with a single command you have to use the command prompt. See below:

```
c:\Temp>NETDOM QUERY FSMO
Schema master               DC01.test.local
Domain naming master        DC01.test.local
PDC                         DC01.test.local
RID pool manager            DC01.test.local
Infrastructure master       DC01.test.local
The command completed successfully.
```

