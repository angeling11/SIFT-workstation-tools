# SIFT Workstation Tools

This is a summary and a compilation of all the tools and usage presented by Rob Lee on the SANS Digital Forensics and Incident Response YouTube channel, you can watch the video [here](https://www.youtube.com/watch?v=ai_7Fkv6igw).

All the rights are by Rob Lee and SANS Institute.



## Getting Started with SIFT

[SIFT Workstation](http://dfir.to/SIFT-WORKSTATION) is a open-source toolkit for forensics examinations. It's a ready to go Linux system.

The system can be installed as a virtual machine appliance on virtualization software or it can be also installed on a physical machine, although on the SIFT website you can download the .ova file to easily set up. I the other hand you can install it manually the system following the manual on the SIFT website. In fact, you can install the system on windows under the WSL (Windows Subsystem for Linux) following the same steps, but some mounting disk images tools don't work properly.

In SIFT you can find tools for examining system, registry, memory and network and much more. Some of those are:

- log2timeline (Timeline Generation Tool)

- Rekall Framework (Memory Analysis)

- Volatility Framework (Memory Analysis)

- 3rd Party Volatility Plugins

- bulk_extractor

- autopsy

- afflib

- afflib-tools

- ClamAV

- dc3dd

- imagemounter

- libbde 

- libesedb

- libevt

- libevtx

- libewf 

- libewf-tools

- libewf-python

- libfvde

- libvshadow

- lightgrep

- log2timeline

- Plaso

- Qemu

- regripper and plugins

- SleuthKit

- among others...

You can list some of there with the following commands:

- "ewf" + Tab
- "reg" + Tab
- "log2timeline" + Tab
- "ls /usr/local/bin/*.py" + Tab

There are some Cheat Sheet aimed to summary some important tools or useful information when you are on a Digital Forensic Investigation, for example, Memory Forensics Cheat Sheet, Hex and Regex Forensics Cheat Sheet, Rekall Cheat Sheet, you can find all of them [here](http://dfir.to/DFIR-CheatSheets).



## Copying Evidence To SIFT

In SANS we have a SMB server, we can just copy the IP adress from the machine, which we consult  with ```ipconfig```, and paste on the file explorer of the host machine with the prefix ```\\``` and then press return,  for example ```\\192.168.1.11``` and we'll be prompted with the network credentials of the SIFT system, after that we'll see two folders ```mnt``` and ```cases``` on which we can copy files that we want to analyze with the tools of the SIFT Workstation.



## Mounting Disk Images

