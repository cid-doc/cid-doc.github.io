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
    - [cid-gtk](#cid-gtk)  
    	- [Join the domain](#join)  
    	      - [Advanced mode](#advanced)  
    	- [Remove from domain](#leave)  
    	- [Change station behavior](#behavior)  
    	- [Block logon](#block)  
    	- [Unblock logon](#unblock)  
    	- [Manage domain accounts in local groups](#account)  
    	- [Manage shares](#shares)  
    	- [Help](#help)  
    - [cid](#cid)
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
The requirements can be easily installed through the package managers of these distributions:

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
After installing the requirements, download the tarball, unzip it and run as **root** the **INSTALL.sh** script. You can use the following commands:

    $ wget http://downloads.sf.net/c-i-d/cid-1.1.2.tar.gz
    $ tar -xzf cid-1.1.2.tar.gz
    $ cd cid-1.1.2
    $ sudo ./INSTALL.sh

>**Note:** Run `sudo ./INSTALL.sh uninstall` to uninstall the program files from the same version of the package.

### Ubuntu and derivatives <a name="Ubuntu" />
In *Ubuntu* and its derivations it is possible to install the CID through packages available in the *PPA* repository. These packages contain the [requirements](#Requirements) marked as dependencies, which allows them to be automatically installed.The following are the commands for installing these packages:

    # add-apt-repository ppa:emoraes25/cid
    # apt update
    # apt install cid cid-gtk


## Features <a name="Features" />
The CID consists of four main executables subdivided into two graphical tools (`cid-gtk` and `cid-change-pass-gtk`) and two command line utilities (`cid` and `cid-change-pass`). Both pairs contain equivalent features and they all accept the following general options as a argument in the command line:  

| Options           | Description               |
| ----------------- | ------------------------- |
| **-v, --version** | Show the version and exit |
| **-h, --help**    | Show the help and exit    |

### cid-gtk <a name="cid-gtk" />
The **cid-gtk** (_GUI_) is the tool that contains the main features of the program. Through it you can insert your Linux computer in an AD domain and later manage a series of functions in the system, within this context.  

The available features are described in the following sections.

#### Join the domain <a name="join" />
This function allows you to join the Linux computer to an AD domain. For that, it is necessary to inform the domain data in the respective fields as shown in the table below:

| Field                   | Description |
| ----------------------- | ----------- |
| **Domain**              | Domain name (FQDN) |
| **Organizational Unit** | Optionally, you can specify an Organizational Unit where the computer account must be created when join it the domain. If the OU is not entered or is not found, the computer account will be created in the default container (computers) |
| **User**                | Domain administrator user |
| **Password**            | User password |
| **Mode**                | Select one of two join modes: **Default** or **Advanced**. _Default_ mode is adopted if no selection is made. [Advanced mode](#advanced) opens a form that allows you to customize the settings and modifications that the CID will perform on the system during the process of joining the domain. All configuration options available in this mode are directly opposite to the settings adopted in the Default mode |

>**Note:** Before modifying the system files, the CID makes a backup of these files in the `/var/lib/cid/backups/ori` directory.

##### Advanced mode <a name="advanced" />
The options available in this mode are:

| Option                                  | Description |
| --------------------------------------- | ----------- |
| **Disable NetBIOS over TCP/IP**         | Disables support for the NetBIOS API implemented by Samba |
| **Disable authentication via Kerberos** | This causes the pam_winbind module to not attempt to obtain the kerberos tickets known as Ticket Granting Tickets (TGTs) of the Authentication Server (AS) during user login |
| **Disable credential caching**          | Disables Samba support for off-line authentication, or authentication with local cached credentials. This requires real-time communication with the authentication server for the logins the domain users |
| **Disable logon scripts**               | This disables [logon scripts](#Logon_scripts) |
| **Do not use domain as default**        | Configures Samba not to use the joined domain as the system default. This makes it necessary to specify the domain name before the user or group name (format: `DOMAIN\user` or `DOMAIN\group`), both in authentication and in system commands that receive user or group names as argument |
| **Enable authentication for "sudo"**    | This requires that domain users who are given administrative privileges on the Linux computer need to authenticate when running the `sudo` command (see [Manage domain accounts in local groups](#account)) |
| **Use "idmap_ad" (RFC 2307)**           | This option allows the use of the **idmap_ad** backend, which implements an API to obtain the Unix attributes of users and groups in the domain through domain controllers, as long as they have NIS extensions enabled. By default, the CID configures winbind to use the **idmap_autorid** backend, which establishes these attributes through a predefined configuration on the local system. When selecting this option, a new form will be presented for configuring the backend with the following fields:<br /><br />- **Initial ID:** Initial value of the range of UIDs and GIDs that will be mapped by the backend. This field is required, and the value assigned must be greater than the IDs already used by local users and groups<br /><br />- **Final ID:** End value of the range of UIDs and GIDs that will be mapped by the backend. When not set, the CID will use a random value based on the value set in the Initial ID field<br /><br />- **winbind nss info:** This defines whether information about the home directory and the shell of domain users should also be obtained from DC with the `rfc2307` option, or whether through a predefined Samba configuration with the `template` option. The **template** option is adopted by default |
| **Share all printers on CUPS**          | Enables automatic sharing of all printers configured on the local **CUPS** server through Samba (_SMB protocol_). This makes it unnecessary to configure individual shares for each printer through the [Manage shares](#shares) option |
| **Use keytab file method**              | Configures Samba to use a dedicated keytab file as an authentication method for Kerberos. The `krb_principal_names` parameter in the [cid.conf](#cid.conf) file can be used to specify principal names that you want to be added to the keytab |
| **Add config file to "Samba"**          | It allows adding a file containing configuration parameters to be attached to the settings made by the CID in the _Global_ section of the samba configuration file (_smb.conf_). The CID will filter the contents of this file so that there are no conflict with the defined parameters by default |

#### Remove from domain <a name="leave" />
This function undoes the modifications made in the system for the computer to [join the domain](#join), and by the use of the other CID functionalities after that join.  

When using it, you can optionally fill in the fields with the credentials of a domain administrator for what the CID try remotely delete the computer account from the AD database. If this fails, the operation of this function will not be affected.

>**Note:** A copy of the files modified during this process is stored in the `/var/lib/cid/backups/mod` directory.

#### Change station behavior <a name="behavior" />
This function allows you to change the options of the [Advanced mode](#advanced) after the system joins an AD domain.

>**Note:** Whenever a change is made through this função, a copy of the affected files in the state before modification is stored in the `/var/lib/cid/backups/mod` directory.

#### Block logon <a name="block" />
This function restricts logon in the system to a specific user or group of the domain. When selecting it you must inform the **Account type** (User or Group) and the **Account name**. If no type is selected, the _User_ type is assumed by default.

#### Unblock logon <a name="unblock" />
This function removes the logon restriction applied by the [block logon](#block) function.

#### Manage domain accounts in local groups <a name="account" />
This function allows you to associate domain user accounts with local system groups so that they can perform specific tasks or routines that require administrative privileges.  
 
The CID uses the groups that the **default user** (usually the first user created on the system) belongs to to determine these groups. You can specify any other user using the `defaultuserid` parameter, and you can also add other specific groups using the `localgroups` parameter in the [cid.conf](#cid.conf) file.  

In the **Add account** option, you must inform the type and name of the account in the same way as when using the [block logon](#block) function.  

Note that on Unix systems there is no group nesting. Therefore, when a group account is added, members of that group are individually associated with local groups as they log on to this system. If you enter an asterisk (*) at the end of the group's name, all its members will be immediately associated. If the user is removed from the domain group, the user will be automatically removed from local groups on a next login to this computer.

The **Remove account** option lists domain accounts added to local groups and allows you to select them for removal.

>**Note:** Members of the **domain admins** group are already associated with local groups by default.

#### Manage shares <a name="shares" />
This function allows you to manage _Samba_ shares intuitively.  

The **Add share** option displays a form where you must enter the arguments to create or update a share.  

Three share modes are available:

| Mode           | Description |
| -------------- | ----------- |
| **Common**     | Mode used by default if the argument is omitted. This mode allows share permissions to be managed locally through the `Rule` argument, or through a remote Windows system using the **Microsoft Management Console** (MMC) |
| **Userfolder** | This mode enables the **homes** section of Samba, which is a special type of file sharing that automatically provides a share with the same name as the user who accesses it. In this mode, the `path` argument must contain the path of the parent directory where the folders for each user share are to be created. If this argument is omitted, the `/home` directory will be assumed by default. If `disk quota` is used, it will be automatically applied to each user directory |
| **Printer**    | Mode used to share a printer configured on the local _CUPS_ server through Samba (_SMB protocol_). This is optional or unnecessary when the `Share all printers on CUPS` option (see [Advanced mode](#advanced)) is enabled. In this mode, the `Path` field should be named after a printer configured in CUPS instead of a directory path. If left blank, this argument will receive the value used in the `Name` field or vice versa. When printer sharing is enabled, a special file share called **print$** will be configured to automatically provide shared printer drivers to Windows clients on the network. The location of the directory to be used for this share can be adjusted using the `prtdrvdir` parameter in the [cid.conf](#cid.conf) file. Driver files must be manually copied to this directory or you must use a Windows system utility to manage the drivers on that print server (visit this <a href="https://wiki.samba.org/index.php/ Setting_up_Automatic_Printer_Driver_Downloads_for_Windows_Clients">link</a> for more information) |

#### Help <a name="help" />

### cid <a name="cid" />
The **cid** is the _CLI_ alternative to cid-gtk, and can be used to run all the features of the graphical tool on the command line or in bash scripts.  

Use `man cid` command to access the complete manual for that utility.  

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
