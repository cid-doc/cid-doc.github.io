CID
===

**License: [GPL-3+](https://sourceforge.net/projects/c-i-d/files/COPYING)**  

[Homepage](https://c-i-d.sourceforge.io) | [Discussion](https://sourceforge.net/p/c-i-d/discussion/) | [Donations](https://sourceforge.net/p/c-i-d/donate)  


## Contents
- Description
- Requirements  
    - Installation of the requirements in the main Linux distributions  
- Installation
- Features  
    - cid and cid-gtk  
    - cid-change-pass and cid-change-pass-gtk  
    - cid.conf  
    - CID Init Script  
- Logon scripts  
    - logon.sh and logon_root.sh  
    - Mapping file shares (shares.xml)  
    - Mapping shared printers  
- Troubleshooting


## Description
**CID** (*Closed In Directory*) is a set of bash scripts for inserting and managing Linux computers in **Active Directory** domains. Modifications made to the system allow Linux to behave like a Windows computer within AD.  

You can do things like:  
- Run logon scripts
- Automatically mount network shares (files and printers)
- Offline logon (Credential cache)
- Easily grant privileges to domain users (eg: access to the sudo command)
- Manage Samba shares (files and printers) through a graphical interface
- Apply disk quota per shared directory (such as Windows Server)


## Requirements
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

### Installation of the requirements in the main Linux distributions

#### Debian (>= 9):
    # apt install passwd sudo acl attr systemd x11-xserver-utils policykit-1 zenity iproute2 iputils-ping keyutils krb5-user libnss-winbind libpam-winbind samba-common-bin samba-dsdb-modules samba-vfs-modules smbclient samba cifs-utils libpam-mount cups-daemon cups-client

#### Fedora (>= 30):
    # dnf install shadow-utils sudo acl attr systemd xorg-x11-server-utils zenity iproute iputils keyutils krb5-workstation gvfs-smb samba-winbind samba-winbind-clients samba-client samba cifs-utils pam_mount cups cups-client

#### OpenSUSE (>= 15):
	# zypper install sudo acl attr systemd xhost zenity iproute2 iputils keyutils krb5-client samba-dsdb-modules gvfs-backend-samba samba-winbind samba-client samba cifs-utils pam_mount cups-client cups

#### CentOS (= 7):
    # yum install epel-release
    # yum install shadow-utils sudo acl attr systemd xorg-x11-server-utils zenity iproute iputils keyutils krb5-workstation gvfs-smb samba-winbind samba-winbind-clients samba-client samba cifs-utils pam_mount cups


## Installation
After installing the requirements, download the source package, unzip it and run as **root** the **INSTALL.sh** script. You can use the following commands:

    $ wget http://downloads.sf.net/c-i-d/cid-1.1.2.tar.gz
    $ tar -xzf cid-1.1.2.tar.gz
    $ sudo cid-1.1.2/INSTALL.sh

**Note 1:** Run `./INSTALL.sh uninstall` to uninstall the program files from the same version of the package.

**Note 2:** In **Ubuntu** and its derivations it is possible to install the CID through packages available in the *PPA* repository. These packages contain the requirements marked as dependencies, which allows them to be automatically installed.The following are the commands for installing these packages:

    # add-apt-repository ppa:emoraes25/cid
    # apt update
    # apt install cid cid-gtk


## Features
The CID consists of four main executables subdivided into two graphical tools and two command line utilities. Both pairs contain equivalent features and they all accept the following general options as a command line argument:  

| Options       | Description               |
| ------------- | ------------------------- |
| -v, --version | Show the version and exit |
| -h, --help    | Show the help and exit    |

### cid and cid-gtk

### cid-change-pass and cid-change-pass-gtk

### cid.conf

### CID Init Script


## Logon scripts

### logon.sh and logon_root.sh

### Mapping file shares (shares.xml)

### Mapping shared printers


## Troubleshooting



>Release 1.1.2 2021-05-15  
>
>*Copyright (C) 2012-2021 Eduardo Moraes <<emoraes25@gmail.com>>*
