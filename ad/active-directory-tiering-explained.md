# Index
- [Index](#index)
- [Introduction](#introduction)
    - [Tier 0 Domain](#tier-0-domain)
    - [Tier 1 Services](#tier-1-services)
    - [Tier 2 End Users](#tier-2-end-users)
    - [The three Commandments](#the-three-commandments)
- [Implementing the AD Administrative Tier Model](#implementing-the-ad-administrative-tier-model)
    - [Authentication Policies](#authentication-policies)
    - [Deep Dive in Authentication Policies](#deep-dive-in-authentication-policies)
    - [FAST (Flexible Authentication Secure Tunneling)](#fast-flexible-authentication-secure-tunneling)
- [Comments](#comments)

# Introduction
The AD Administrative Tier Model prevents escalation of privilege by restricting what Administrators can control and where they can log on. In the context of protecting Tier 0, the latter ensures that Tier 0 credentials cannot be exposed to a system belonging to another Tier (Tier 1 or Tier 2).

### Tier 0 Domain
The first tier is your domain tier and also your most sensitive tier where most effort is put in to protect it. This is where your domain controllers reside and your certificate infrastructure (PKI).

### Tier 1 Services
In the second tier all your services will reside. Think of web servers, file servers or other services. These are the services used by your end users.

### Tier 2 End Users
The last tier is for the end users and their devices. This tier is the most exposed to the outside world and consequently the most dangerous tier as well. It means we need to be careful what we access with these devices and prevent using these devices to manage our sensitive assets within our network.

### The three Commandments
There are 3 rules, or call them commandments which are crucial for a tiering model to function. The three commandments are:
- **Credentials from a higher-privileged tier** (e.g. Tier 0 Admin or Service account) **must not be exposed to lower-tier systems** (e.g. Tier 1 or Tier 2 systems).
- **Lower-tier credentials can use services provided by higher-tiers, but not the other way around**. E.g. Tier 1 and even Tier 2 system still must be able to apply Group Policies.
- **Any system or user account that can manage a higher tier is also a member of that tier, whether originally intended or not.**  

When you follow these 3 rules you can reduce your attack surface a lot. The picture below explains how these 3 rules work.  
<sub>*Microsoft Tiering Model*</sub>  
![alt text](https://techcommunity.microsoft.com/t5/s/gxcuf89792/images/bS00MDUyODUxLTU1MDI2Mmk4RTYwNjEwMzI1QUM3RTJB?image-dimensions=450x450&revision=21)  
<sub>*Src: Microsoft Community Hub*</sub> 

# Implementing the AD Administrative Tier Model
In most guides for deploying an AD tiering model you will read about complicated GPO setups that prevent higher tiered administrative accounts to login on lower tiered assets. This approach comes with a couple of downsides.
- Group policies can be bypassed by administrators
- Only domain joined assets are affected  

### Authentication Policies
To counter this there is a solution called *auhentication policies*, authentication Policies provide a way to contain high-privilege credentials to systems that are only pertinent to selected users, computers, or services. With these capabilities, you can limit Tier 0 account usage to Tier 0 hosts. That’s exactly what we need to achieve to protect Tier 0 identities from credential theft-based attacks.  

> With Kerberos Authentication Policies you can define a claim which defines where the user is allowed to request a Kerberos Granting Ticket from.

### Deep Dive in Authentication Policies
Authentication policies work on *Kerberos* level, allowing to set restriction on authentication protocol level. More specifically, the Kerberos extension called FAST (Flexible Authentication Secure Tunneling) is used by Authentication policies. 

### FAST (Flexible Authentication Secure Tunneling)
FAST provides a protected channel between the Kerberos client and the KDC for the whole pre-authentication conversation by encrypting the pre-authentication messages with a so-called armor key and by ensuring the integrity of the messages.
