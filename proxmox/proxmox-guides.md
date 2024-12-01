# Index

- [Index](#index)
- [Introduction](#introduction)
- [Remove subscription banner](#remove-subscription-banner)

# Introduction
This article contains many Proxmox guides deriving from the notes I have taken over the years using Proxmox. I love proxmox for its rich set of features and obviously because its open source. These are short guides and longer guides will be in separate articles.

# Remove subscription banner
By default Proxmox shows a banner when logging in stating that you do not have a subscription. This is annoying when you are using it without a subscription. To remove it follow the following steps:
- Login to your Proxmox instance, either on the Web GUI, or using SSH. In the Web GUI go to the host and then **Shell**
- We need to edit the following file: ```proxmoxlib.js```, but first we'll create a backup, run the following command:
```
root@proxnode02:~# cp /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js.bak
root@proxnode02:~#
```
- Now we can start to edit the file, run the following command:
```
nano /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js
```
- This will open the file in the Nano text editor.
