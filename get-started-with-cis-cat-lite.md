# Index

- [Index](#index)
- [Introduction](#introduction)
- [1 Download Cis Cat Lite](#1-download-cis-cat-lite)
    - [Links](#links)
    - [Download contents](#download-contents)
- [2 Run assesment from GUI](#2-run-assesment-from-gui)
    - [Basic (Local System)](#basic-local-system)
    - [Advanced (Remote Systems)](#advanced-remote-systems)
- [3 Run Assesment from CLI](#3-run-assesment-from-cli)
- [4 Example powershell script for automatic assesment](#4-example-powershell-script-for-automatic-assesment)
    - [Script](#script)

# Introduction
CIS-CAT Lite is the free assessment tool developed by the CIS (Center for Internet Security, Inc.). CIS-CAT Lite helps users implement secure configurations for multiple technologies. With unlimited scans available via CIS-CAT Lite, your organization can download and start implementing CIS Benchmarks in minutes.

<p><a href="https://learn.cisecurity.org/cis-cat-lite?wvideo=n6ma0g34sb"><img src="https://embed-ssl.wistia.com/deliveries/a36f9e2e75a1de58a101a76ca7475ee6.jpg?image_play_button_size=2x&amp;image_crop_resized=960x540&amp;image_play_button=1&amp;image_play_button_color=0086bfe0" width="400" height="225" style="width: 400px; height: 225px;"></a></p><p><a href="https://learn.cisecurity.org/cis-cat-lite?wvideo=n6ma0g34sb">CIS-CAT Lite</a></p>

- Instantly check your systems against CIS Benchmarks.
- Receive a compliance score 1-100.
- Follow remediation steps to improve your security.

# 1 Download Cis Cat Lite
In order to download Cis Cat Lite for free you do need to register with an email adress and a link will be sent to you with wich you can download Cis Cat Lite.  

### Links
[Download Cis Cat Lite](https://learn.cisecurity.org/cis-cat-lite) | *Version as of writing: 4.46.0*  
[CIS CAT Documentation](https://ciscat-assessor.docs.cisecurity.org/en/latest/)
<br></br> 
Extract all contents from the ZIP file to al location from where you want to run the assesment. For example: c:\temp.

### Download contents
```
PS C:\temp\CIS-CAT Lite Assessor v4.46.0\Assessor> ls


    Directory: C:\temp\CIS-CAT Lite Assessor v4.46.0\Assessor


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----        11/20/2024  12:08 PM                benchmarks
d-----        11/20/2024  12:10 PM                config
d-----         9/24/2024  10:08 PM                custom
d-----        11/20/2024  12:08 PM                documentation
d-----        11/20/2024  12:08 PM                jre
d-----        11/20/2024  12:08 PM                lib
d-----         9/24/2024  10:08 PM                license
d-----        11/20/2024  12:10 PM                logs
d-----        11/20/2024  12:08 PM                misc
d-----        11/20/2024  12:12 PM                reports
d-----        11/20/2024  12:08 PM                sce
d-----        11/20/2024  12:08 PM                scripts
d-----        11/20/2024  12:08 PM                setup
-a----        11/20/2024  12:08 PM            751 Assessor-CLI.bat
-a----        11/20/2024  12:08 PM         106246 Assessor-CLI.jar
-a----        11/20/2024  12:08 PM           1059 Assessor-CLI.sh
-a----        11/20/2024  12:08 PM       47314304 Assessor-GUI.exe
-a----        11/20/2024  12:08 PM           1620 README
```

# 2 Run assesment from GUI
When extracted and you open the *assesor* folder you should have a file named *Assesor-GUI.exe*. This will open the GUI from where you can choose to do a local assesment or a remote one.

> You should have local administrator privelidges to run the assement

When you are hit with the dialog "Do you want to allow this app to make changes to your device?" click *yes*. 

### Basic (Local System)

**Benchmarks**  
The Basic option allows you to do an assesment on the local system you are running the Assesor on. Select the benchmark you want to run, for example *CIS Microsoft Windows 11 Enterprise Benchmark v3.0.0* and select the correct profile. Profiles are a set of checks from the benchmark selected. Taking the windows 11 benchmark the following profiles are available:

| Level | Description |
|:------|:------------|
| Level 1 (L1) | Corporate/Enterprise Environment (General Use) |
| Level 1 (L1) + (BL) | L1 + Bitlocker |
| Level 2 (L2) | High Security/Sensitive Data Environment (Limited Functionality) |
| Level 2 (L2) + (BL) | L2 + Bitlocker |
| Bitlocker (BL) | optional add-on for when Bitlocker is deployed |

Choose one of the profiles to start the assesment, typically Level 1 + Bitlocker will be used for normal office assets. Next click Add. You can add multiple benchmarks by repeating the process. For example you can also add the *CIS Google Chrome Benchmark v3.0.0*. This will run both benchmarks.

> When running the assesment a temporary folder is used to store files for the assement activities. You can change this location before running the assesment. By default the users temp directory is used: C:\Users\<User>\AppData\Local\Temp

**Report Output Options**  
Click next, now you can choose the file formats you want the report to be in. These are all the options:

| Format | Description |
|:-------|:------------|
| HTML | (Default) Report in HTML format |
| CSV | Comma Separated Value |
| Text | Plain text format |
| ARF XML | Asset Reporting Format (ARF) |
| JSON | Javascript Object Notation |

Choose folder to save the report files. By default the reports are saved in the *reports* folder in the *Assesor* folder from where you run the cis cat gui.
> C:\temp\CIS-CAT Lite Assessor v4.46.0\Assessor\reports

You can also POST the results to an API endpoint. This could be the CIS CAT Dashboard, or your own web application, optionally you can also ignore SSL warnings.
<br></br>

**Logging Options**  
By default the log level is set to WARN, so that warnings and errors are logged. For debugging purpose you can set the logging level to:
- Info
- Debug
- Trace
- All Messages
  
**Configuration Output Options**  
You can choose to save the configuration to a XML file for reuse later. Optionally you can also encrypt the file so a user password is needed.

Click next and confirm to start the assesment. You can view the process and check for any errors. When done the reports will be available in the selected folder. 

### Advanced (Remote Systems)
If you want to run assesments on remote systems initiated from one system you can use the advanced option. In this example I will focus on remote Windows devices.
<br></br>

**Information**
In this section you have to enter information about the remote system. The following table shows all fields that need to be filled based on Windows Systems.

| Field | Description |
|:------|:------------|
| Target System Name | This is the name used to differentiate the benchmarks |
| Target System Type | Type of system, in this case we choose *Windows* |
| Target System Protocol for WinRM | Choose between HTTP (No Cert and port 5985) or HTTPS (Cert and port 5986 or custom), consult your WinRM configuration|
| Port | WinRM port |
| Username | User to connect to remote system, AD Domain user example: user@domain.local, this user should have local administrator privileges |
| Password | Password for the above user, password is saved in encrypted configuration file |
| IP Address / Hostname | IP Address of the remote host or DNS hostname |
| Temporary Path | *Optional* If you want to change the temp folder on the remote host |

When all information has been entered you can select the benchmark and profile you want to run and click *Save* to add the remote host. In the next screen you can you'll see a list of target hosts added, and by clicking *Add* and repeat the process for another host. When all desired hosts are added you can proceed by clicking *next* and you can set the **Report Output Options**, please refer to **Report Output Options** section in the **Basic (Local System)** part of this chapter.  
<br></br>
Next you can start the assesment same why as in the basic method. For each host configure reports will be generated according to the option you configured. 

# 3 Run Assesment from CLI
If you want to automate assements or integrate it within a script you will have to use the CLI options to run a benchmark. This chapter will go over the steps needed to run a benchmark with the CLI utility.
<br></br>
To run the CLI utility within Windows you have to use the *Assessor-CLI.bat* file. Running this without any parameters will give you an overview of all parameters and options you can set.

```
c:\temp\CIS-CAT Lite Assessor v4.46.0\Assessor>Assessor-CLI.bat

--------------------------------------------------------------------------------------
   ,o88888o.    8888    d888888o.          ,o88888o.           8.    8888888888888888
  8888    `88.  8888  .`8888:' `88.       8888    `88.        .88.         8888
,88888      `8. 8888  8.`8888.   Y8     ,88888      `8.      .8888.        8888
888888          8888  `8.`8888.         888888              .`88888.       8888
888888          8888   `8.`8888.   888  888888             .8.`88888.      8888
888888          8888    `8.`8888.  888  888888            .8`8.`88888.     8888
888888          8888     `8.`8888.      888888           .8' `8.`88888.    8888
`88888      .8' 8888 8b   `8.`8888.     `88888      .8' .8'   `8.`88888.   8888
  8888    ,88'  8888 `8b.  ;8.`8888       8888    ,88' .888888888.`88888.  8888
   `888888P'    8888  `Y8888P ,88P'        `888888P'  .8'       `8.`88888. 8888
--------------------------------------------------------------------------------------
         Welcome to CIS-CAT Pro Assessor; built on 09/24/2024 21:10 PM
--------------------------------------------------------------------------------------
  This is the Center for Internet Security Configuration Assessment Tool, v4.46.0
          At any time during the selection process, enter 'q!' to exit.
--------------------------------------------------------------------------------------

Verifying application

usage: Assessor-CLI.[bat|sh] -[options] <extras>
---------------------------------------------------------------------------------------------------
 Options                                           Tip
---------------------------------------------------------------------------------------------------
 -b,--benchmark <BMK-OR-DSC>                       Path to file containing assessment content, such
                                                   as an XCCDF or DataStream Collection
...
```
> Note: You need to run the assesor with local administrative privileges. Otherwise the script will fail.

**Interactive Mode**  
You can run the assesor in interactive mode which allows you to choose which benchmark and profile to run. To do this run the assesor bat file with the **-i** option.

```
c:\temp\CIS-CAT Lite Assessor v4.46.0\Assessor>Assessor-CLI.bat -i

Verifying application

Attempting to load the default sessions.properties, bundled with the application.
Started Assessment 1/1

Loading Benchmarks/Data-Stream Collections

Available Benchmarks/Data-Stream Collections:
 1. CIS Controls Assessment Module - Implementation Group 1 for Windows 10 v1.0.3
 2. CIS Controls Assessment Module - Implementation Group 1 for Windows Server v1.0.0
 3. CIS Google Chrome Benchmark v3.0.0
 4. CIS Microsoft Windows 10 Enterprise Benchmark v3.0.0
 5. CIS Microsoft Windows 10 Stand-alone Benchmark v3.0.0
 6. CIS Microsoft Windows 11 Enterprise Benchmark v3.0.0
 7. CIS Ubuntu Linux 20.04 LTS Benchmark v2.0.1
 > Select Content # (max 7):
```
The application will guide you through the process.
<br></br>

**Automated method**  
To run an automated assesment you need to define the benchmark and profile to run. In this example we will use the Windows 11 benchmark with level 1 profile and bitlocker enabled.

```
c:\temp\CIS-CAT Lite Assessor v4.46.0\Assessor>Assessor-CLI.bat -b "benchmarks\CIS_Microsoft_Windows_11_Enterprise_Benchmark_v3.0.0-xccdf.xml" -p "Level 1 (L1) + BitLocker (BL)"
```

# 4 Example powershell script for automatic assesment
To run this script you need a file share where you can host the CIS CAT Lite files and set permissions so the user account who performs the assesment can access the files and write reports to it. The script will download the required files to a temporary location and execute the assesment. When the assesment is finished the report will be stored on the file share and the temporary files will be deleted again.

### Script

```

```