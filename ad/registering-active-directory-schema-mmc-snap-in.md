# Index
- [Index](#index)
- [Introduction](#introduction)
- [Register Snap-in](#register-snap-in)
- [Register Snap-in on non Domain Controler](#register-snap-in-on-non-domain-controler)
- [Open snap-in](#open-snap-in)
- [Comments](#comments)

# Introduction
Most of the Active Directory MMC snap-ins do not require that you manually register the associated DLL. Microsoft requires this with the Active Directory Schema snap-in due to the sensitive nature of modifying the schema.  
<br></br>
The schmmgmt.dll file is installed as part of adminpak.msi or when a domain controller is promoted.

# Register Snap-in
Open a command promt with administrative privileges and run the following command:
```
C:\Windows\system32>regsvr32 schmmgmt.dll
```
If the command was succesfull a dialog windows should show with the message: *DLLRegisterServer in schmmgmt.dll succeeded.*

# Register Snap-in on non Domain Controler
If you wish to run the snap-in on a non domain controller you need to install the adminpak.msi package or register the full path of the *schmmgmt.dll* file which can be found in the \i386 directory of a Windows Server ISO / CD.

# Open snap-in
To open the Active Directory Schema snap-in take the following steps:
- Press Windows + R and type MMC, press yes when asked for elevation
- In the MMC console open the file menu and choose *Add/Remove Snap-in...*
- Select *Active Directory Schema* and click add, then OK

This opens up the schema snap-in and from here you can perform actions on the AD schema.
