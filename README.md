# SIFT Workstation Tools

This is a summary and a compilation of all the tools and usage presented by Rob Lee on the SANS Digital Forensics and Incident Response YouTube channel, you can watch the video [here](https://www.youtube.com/watch?v=ai_7Fkv6igw), which pretends to be just a Get Started walkthrought.

This document just recompiles all the tools and usage described in the video, these tools that can be installed in any other system, but are installed by default in SIFT Workstation. Also, there are a bunch of other tools don't listed but are installed.

 

## Getting Started with SIFT

[SIFT Workstation](http://dfir.to/SIFT-WORKSTATION) is a open-source toolkit for forensics examinations in a ready to go Linux system.

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

- "ewf" + *Tab*
- "reg" + *Tab*
- "log2timeline" + *Tab*
- "ls /usr/local/bin/"

There are some Cheat Sheet aimed to summary some important tools or useful information when you are on a Digital Forensic Investigation, for example, Memory Forensics Cheat Sheet, Hex and Regex Forensics Cheat Sheet, Rekall Cheat Sheet, you can find all of them [here](http://dfir.to/DFIR-CheatSheets).



## Copying Evidence To SIFT

In SANS we have a SMB server, we can just copy the IP address from the machine, which we consult  with `ipconfig`, and paste on the file explorer of the host machine with the prefix `\\` and then press return,  for example `\\192.168.1.11` and we'll be prompted with the network credentials of the SIFT system, after that we'll see two folders `mnt` and `cases` on which we can copy files that we want to analyze with the tools of the SIFT Workstation.



## Mounting Disk Images

The first step is to go to the folder where the image disk is and get a root account with `sudo su`. Next we can mount a raw image (*E01* or *Expert Witness Format*) in the system, it can be done with the following command:

`ewfmount <image> <mount point>` , for example: `ewfmount image.e01 /mnt/awf_image`

*EWF* files are a type of disk image, that contains the contents and structure of a storage device, a disk volume and even physical memory.

Due to that the last command just mount a raw image by itself, actually it creates a file called *ewf1*, this image is located at the folder specified in the *ewfmount* mount, now the file system contained on that image have to be mounted too, that can be done with the following command:

`mount -o ro,loop,show_sys_files,streams_interface=windows <ewf image> <mount point>`

It's a good idea, if isn't already, set the mount command as an alias.

Now the file system can be seen in the mount folder, including files that are not commonly visible in the OS.

Finally, after the image have been analyzed, both images (ewf and file system) need to be unmounted, this can be done like follows:

`umount <mount point>`



### *imageMounter.py*

An easiest way to do all this process is by using the imageMounter.py script, which can do all this steps automatically, mount *ewf* and file system images, as simple as:

`imageMounter.py -s <image> <file system mount point>`

This command will display all the info related to the process of mounting to see where the images where mounted and how to unmount them.



This tool and `bdmount` can be provided with a password to mount a Bit Locker or a encrypted image. For this precess the key has to be known, otherwise, it could be brute forced.



## Volume Shadow Examinations

Volume shadows are like a backup of the logical partition in the windows systems, usually activated by default and it creates snapshots regularly, which can be useful because some deleted files can be stored there.

The first step is to mount the ewf image (*E01*) and go to the ewf1 file path. To see the snapshots or volume shadow and related info, such as creation time, volume size and others, the next command can be executed:

`vshadowinfo ewf1`

To mount the volume shadow snapshots the following command is used:

`vshadowmount ewf1 /mnt/vss/`

`/mnt/vss` is a default folder in SIFT Workstation for this purpose.

Once run this command, all the snapshots can be listed in that folder, each of them correspond to a snapshot, and each one needs also be mounted to see its content. For that it's used the default folder `/mnt/shadow_mount/` which contains twenty default sub-folders for up to twenty volume shadow files. A file can be mounted as follows:

`mountwin vss* /mount/shadow_mount/vss*`

**\*** here represents the volume shadow's number

In case there was a lot of volume shadow files, this process can be automated with a for loop from the `/mnt/vss` folder as follows:

`for i in vss*; do mountwin $i /mnt/shadow_mount/$i; done`

If an error is showed, it means that a file is corrupted, but in the `/mnt/shadow_mount` we can check which files were mounted.



## Creating Timelines via SIFT

SIFT Workstation also includes the *Plaso* (*log2timeline*) framework, which creates timelines. It extracts timestamps from the files found in a forensics image.

We can run it in the *ewf1* files as follows:

`log2timeline.py /cases/<output>.dump ewf1`

And the it will prompt us for the volume shadow files we want to extract from, detailed instructions are described right there.

It might take a long time, depends of the size of the image, this due to the fact that all the files are analyzed.

The output can be formated with `psort.py`, for detailed information, go to the documentation or to the SIFT [Plaso Cheat Sheet](https://digital-forensics.sans.org/media/Plaso-Cheat-Sheet.pdf).



## Memory Analysis via SIFT

SIFT has installed *Rekall* and *Volatility* for this purpose.

### *Volatility*

For *Volatitility*, there are a lot of plugins that can be installed to extend its functionalities.

To use *Volatility*, the first thing to do is to check for the profile we're been using, this can be determined, using:

`volatitlity -f <image file> imageinfo`

Here we can see some profiles, and it's necessary to determine which one to use, then we can analyze it with plugins with the following structure:

`volatitlity -f <image file> --profile=<profile> <plugin>`

We can list the available plugins or consult information about the usage of certain plugin:

`volatility --info`

`volatility <plugin> -h`



## Registry Examinations

This is refereed to analyze the Windows *registry hives*, such as the *NTUSER.DAT*, *SAM*, *SECURITY*, and other well known registry files described in the next table:

Registry | Description | Location
--- | --- | ---
NTUSER | Local configuration of a certain account. | User folder 
SAM | "Security Accounts Manager", it contains all accounts configuration, such as username, unique id and cryptographic hash of user's account. | /Windows/System32/config 
SECURITY | It stores the file permissions a user can access and enforce the security police applicable to a certain user. | /Windows/System32/config 
SYSTEM | Contains information about the windows system setup. | /Windows/System32/config 
SOFTWARE | It stores software and windows settings. | /Windows/System32/config 

### *RegRipper*

One of the most used tools is *RegRipper*.

*RegRipper* is a tool, which plugins need to be updated frequently. We can run it with:

`rip.pl -r <registry hive> -f <plugin>`

For further information and list of files, can be run with the flag for help: `rip.pl -h`.

### *ShimCacheParser.py*

*ShimCacheParser* is a third party tool which can extract out data from registry hives. This can be used for look for potential identifying malware, and its basic syntax it's as follows:

`ShimCacheParser.py -i <registry hive> --bom`

The last command it's used to extract the data of a registry hive and parse it as a *.csv* file, detailed information can be consulted with: `ShimCacheParser.py -h`.

