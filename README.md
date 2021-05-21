CID
===

**License: [GPL-3+](https://sourceforge.net/projects/c-i-d/files/COPYING)**  

[Homepage](https://c-i-d.sourceforge.io) | [Discussion](https://sourceforge.net/p/c-i-d/discussion/) | [Donations](https://sourceforge.net/p/c-i-d/donate)  


## Contents
- [Description](#Description)
- [Requirements](#Requirements)  
    - [Installation of the requirements in the main Linux distributions](#Install_Requirements)  
        - [Debian](#Debian)
        - [Fedora](#Fedora)
        - [OpenSUSE](#OpenSUSE)  
        - [CentOS](#CentOS)  
- [Installation](#Installation)
    - [Ubuntu and derivatives](#Ubuntu)  
- [Features](#Features)  
    - [cid and cid-gtk](#cid_cid-gtk)  
    	- [Join the domain](#join)  
    	- [Remove from domain](#leave)  
    	- [Change station behavior](#behavior)  
    	- [Block logon](#block)  
    	- [Unblock logon](#unblock)  
    	- [Manage domain accounts in local groups](#account)  
    	- [Manage shares](#shares)  
    	- [Help](#help)  
    - [cid-change-pass and cid-change-pass-gtk](#ccp_ccp-gtk)  
    - [cid.conf](#cid.conf)  
    - [CID Init Script](#CIS)  
- [Logon scripts](#Logon_scripts)  
    - [logon.sh and logon_root.sh](#logon_lroot.sh)  
    - [Automatic mapping of file shares](#map_shares)
    - [Automatic mapping of shared printers](#map_printers)  
- [Troubleshooting](#Troubleshooting)


## Description <a name="Description" />
**CID** (*Closed In Directory*) is a set of bash scripts for inserting and managing Linux computers in **Active Directory** domains. Modifications made to the system allow Linux to behave like a Windows computer within AD.  

You can do things like:  
- Run logon scripts
- Automatically mount network shares (files and printers)
- Offline logon (credential cache)
- Easily grant privileges to domain users (eg: access to the `sudo` command)
- Manage **Samba** shares (files and printers) through a graphical interface
- Apply disk quota per shared directory (such as Windows Server)


## Requirements <a name="Requirements" />
- bash (>= 4)
- awk (= any)
- {diff,find,core}utils (= any)
- grep (= any)
- sed (= any)
- gzip (= any)
- hostname (= any)
- mount (= any)
- passwd (= any)
- systemd (= any)
- acl (= any)
- attr (= any)
- sudo (= any)
- pkexec [polkit] (= any)
- xhost (= any)
- zenity (>= 3.18.1)
- iproute[2] (= any)
- ping (= any)
- keyutils (= any)
- Kerberos V5 (>= 1.13)
- Samba with Winbind (>= 4.3.11)
- cifs-utils (>= 6.4)
- pam_mount (>= 2.14)
- CUPS [server and clients tools] (= any)

### Installation of the requirements in the main Linux distributions <a name="Install_Requirements" />
The requirements of the CID can be easily installed through the package managers of these distributions:

#### Debian <a name="Debian" />
    # apt install passwd sudo acl attr systemd x11-xserver-utils policykit-1 zenity iproute2 iputils-ping keyutils krb5-user libnss-winbind libpam-winbind samba-common-bin samba-dsdb-modules samba-vfs-modules smbclient samba cifs-utils libpam-mount cups-daemon cups-client

#### Fedora <a name="Fedora" />
    # dnf install shadow-utils sudo acl attr systemd xorg-x11-server-utils zenity iproute iputils keyutils krb5-workstation gvfs-smb samba-winbind samba-winbind-clients samba-client samba cifs-utils pam_mount cups cups-client

#### OpenSUSE <a name="OpenSUSE" />
	# zypper install sudo acl attr systemd xhost zenity iproute2 iputils keyutils krb5-client samba-dsdb-modules gvfs-backend-samba samba-winbind samba-client samba cifs-utils pam_mount cups-client cups

#### CentOS <a name="CentOS" />
    # yum install epel-release
    # yum install shadow-utils sudo acl attr systemd xorg-x11-server-utils zenity iproute iputils keyutils krb5-workstation gvfs-smb samba-winbind samba-winbind-clients samba-client samba cifs-utils pam_mount cups


## Installation <a name="Installation" />
After installing the requirements, download the source package, unzip it and run as **root** the **INSTALL.sh** script. You can use the following commands:

    $ wget http://downloads.sf.net/c-i-d/cid-1.1.2.tar.gz
    $ tar -xzf cid-1.1.2.tar.gz
    $ sudo cid-1.1.2/INSTALL.sh

>**Note:** Run `sudo cid-1.1.2/INSTALL.sh uninstall` to uninstall the program files from the same version of the package.

### Ubuntu and derivatives <a name="Ubuntu" />
In *Ubuntu* and its derivations it is possible to install the CID through packages available in the *PPA* repository. These packages contain the [requirements](#Requirements) marked as dependencies, which allows them to be automatically installed.The following are the commands for installing these packages:

    # add-apt-repository ppa:emoraes25/cid
    # apt update
    # apt install cid cid-gtk


## Features <a name="Features" />
The CID consists of four main executables subdivided into two graphical tools (`cid-gtk` and `cid-change-pass-gtk`) and two command line utilities (`cid` and `cid-change-pass`). Both pairs contain equivalent features and they all accept the following general options as a command line argument:  

| Options       | Description               |
| ------------- | ------------------------- |
| -v, --version | Show the version and exit |
| -h, --help    | Show the help and exit    |

### cid and cid-gtk <a name="cid_cid-gtk" />

#### Join the domain <a name="join" />

#### Remove from domain <a name="leave" />

#### Change station behavior <a name="behavior" />

#### Block logon <a name="block" />

#### Unblock logon <a name="unblock" />

#### Manage domain accounts in local groups <a name="account" />

#### Manage shares <a name="shares" />

#### Help <a name="help" />

### cid-change-pass and cid-change-pass-gtk <a name="ccp_ccp-gtk" />

### cid.conf <a name="cid.conf" />

### CID Init Script <a name="CIS" />


## Logon scripts <a name="Logon_scripts" />

### logon.sh and logon_root.sh <a name="logon_lroot.sh" />

### Automatic mapping of file shares <a name="map_shares" />

### Automatic mapping of shared printers <a name="map_printers" />


## Troubleshooting <a name="Troubleshooting" />



>Release 1.1.2 2021-05-15  
>
>*Copyright (C) 2012-2021 Eduardo Moraes <<emoraes25@gmail.com>>*
