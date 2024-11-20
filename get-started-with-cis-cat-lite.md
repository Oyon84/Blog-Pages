# Index

- [Index](#index)
- [Introduction](#introduction)
- [1 Download Cis Cat Lite](#1-download-cis-cat-lite)
- [2 Run assesment from GUI](#2-run-assesment-from-gui)
    - [Basic (Local System)](#basic-local-system)

# Introduction
CIS-CAT Lite is the free assessment tool developed by the CIS (Center for Internet Security, Inc.). CIS-CAT Lite helps users implement secure configurations for multiple technologies. With unlimited scans available via CIS-CAT Lite, your organization can download and start implementing CIS Benchmarks in minutes.

<p><a href="https://learn.cisecurity.org/cis-cat-lite?wvideo=n6ma0g34sb"><img src="https://embed-ssl.wistia.com/deliveries/a36f9e2e75a1de58a101a76ca7475ee6.jpg?image_play_button_size=2x&amp;image_crop_resized=960x540&amp;image_play_button=1&amp;image_play_button_color=0086bfe0" width="400" height="225" style="width: 400px; height: 225px;"></a></p><p><a href="https://learn.cisecurity.org/cis-cat-lite?wvideo=n6ma0g34sb">CIS-CAT Lite</a></p>

- Instantly check your systems against CIS Benchmarks.
- Receive a compliance score 1-100.
- Follow remediation steps to improve your security.

# 1 Download Cis Cat Lite
In order to download Cis Cat Lite for free you do need to register with an email adress and a link will be sent to you with wich you can download Cis Cat Lite.  
[Download Cis Cat Lite](https://learn.cisecurity.org/cis-cat-lite) Version as of writing: 4.46.0  
Extract all contents from the ZIP file to al location from where you want to run the assesment. For example: c:\temp.
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
<br></br>
**Configuration Output Options**  
You can choose to save the configuration to a XML file for reuse later. Optionally you can also encrypt the file so a user password is needed.

Click next and confirm to start the assesment. You can view the process and check for any errors. When done the reports will be available in the selected folder. 