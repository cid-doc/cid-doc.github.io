CID
===

**License: [GPL-3+](https://sourceforge.net/projects/c-i-d/files/COPYING)**  

[Homepage](https://c-i-d.sourceforge.io) | [Discussion](https://sourceforge.net/p/c-i-d/discussion/) | [Donations](https://sourceforge.net/p/c-i-d/donate) | [Changelog](https://sourceforge.net/projects/c-i-d/files/docs/CHANGELOG)  


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
              - - -> [Advanced mode](#advanced)  
    	- [Remove from domain](#leave)  
    	- [Change station behavior](#behavior)  
    	- [Block logon](#block)  
    	- [Unblock logon](#unblock)  
    	- [Manage domain accounts in local groups](#account)  
    	- [Manage shares](#shares)  
              - - -> [Common mode](#common)  
              - - -> [Userfolder mode](#userfolder)  
              - - -> [Printer mode](#printer)  
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
    - [Hostname longer than 15 characters](#Hostname)
    - [Graphic Mode Login Managers](#Login)

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

| Options           | Description                |
| ----------------- | -------------------------- |
| **-v, --version** | Show the version and exit. |
| **-h, --help**    | Show the help and exit.    |

### cid-gtk <a name="cid-gtk" />
The **cid-gtk** (_GUI_) is the tool that contains the main features of the program. Through it you can insert your Linux computer in an AD domain and later manage a series of functions in the system, within this context.  

The available features are described in the following sections.

#### Join the domain <a name="join" />
This function allows you to join the Linux computer to an AD domain. For that, it is necessary to inform the domain data in the respective fields as shown in the table below:

| Field                   | Description |
| ----------------------- | ----------- |
| **Domain**              | Domain name (FQDN). |
| **Organizational Unit** | Optionally, you can specify an Organizational Unit where the computer account must be created when join it the domain. If the OU is not entered or is not found, the computer account will be created in the default container (computers). |
| **User**                | Domain administrator user. |
| **Password**            | User password. |
| **Mode**                | Select one of two join modes: **Default** or **Advanced**. _Default_ mode is adopted if no selection is made. [Advanced mode](#advanced) opens a form that allows you to customize the settings and modifications that the CID will perform on the system during the process of joining the domain. All configuration options available in this mode are directly opposite to the settings adopted in the Default mode. |

>**Note:** Before modifying the system files, the CID makes a backup of these files in the `/var/lib/cid/backups/ori` directory.

##### Advanced mode <a name="advanced" />
The options available in this mode are:

| Option                                  | Description |
| --------------------------------------- | ----------- |
| **Disable NetBIOS over TCP/IP**         | Disables support for the NetBIOS API implemented by Samba. |
| **Disable authentication via Kerberos** | This causes the pam_winbind module to not attempt to obtain the kerberos tickets known as Ticket Granting Tickets (TGTs) of the Authentication Server (AS) during user login. |
| **Disable credential caching**          | Disables Samba support for off-line authentication, or authentication with local cached credentials. This requires real-time communication with the authentication server for the logins the domain users. |
| **Disable logon scripts**               | This disables [logon scripts](#Logon_scripts). |
| **Do not use domain as default**        | Configures Samba not to use the joined domain as the system default. This makes it necessary to specify the domain name before the user or group name (format: `DOMAIN\user` or `DOMAIN\group`), both in authentication and in system commands that receive user or group names as argument. |
| **Enable authentication for "sudo"**    | This requires that domain users who are given administrative privileges on the Linux computer need to authenticate when running the `sudo` command (see [Manage domain accounts in local groups](#account)). |
| **Use "idmap_ad" (RFC 2307)**           | This option allows the use of the **idmap_ad** backend, which implements an API to obtain the Unix attributes of users and groups in the domain through domain controllers, as long as they have NIS extensions enabled. By default, the CID configures winbind to use the **idmap_autorid** backend, which establishes these attributes through a predefined configuration on the local system. When selecting this option, a new form will be presented for configuring the backend with the following fields:<br /><br />- **Initial ID:** Initial value of the range of UIDs and GIDs that will be mapped by the backend. This field is required, and the value assigned must be greater than the IDs already used by local users and groups<br /><br />- **Final ID:** End value of the range of UIDs and GIDs that will be mapped by the backend. When not set, the CID will use a random value based on the value set in the Initial ID field<br /><br />- **winbind nss info:** This defines whether information about the home directory and the shell of domain users should also be obtained from DC with the `rfc2307` option, or whether through a predefined Samba configuration with the `template` option. The **template** option is adopted by default. |
| **Share all printers on CUPS**          | Enables automatic sharing of all printers configured on the local **CUPS** server through Samba (_SMB protocol_). This makes it unnecessary to configure individual shares for each printer through the [Manage shares](#shares) option. |
| **Use keytab file method**              | Configures Samba to use a dedicated keytab file as an authentication method for Kerberos. The `krb_principal_names` parameter in the [cid.conf](#cid.conf) file can be used to specify principal names that you want to be added to the keytab. |
| **Add config file to "Samba"**          | It allows adding a file containing parameters to be attached to the settings made by the CID in the _Global_ section of the samba configuration file (_smb.conf_). The contents of this file will be filtered so that there is no conflict with the parameters defined by default. |

#### Remove from domain <a name="leave" />
This function undoes the modifications made in the system for the computer to [join the domain](#join), and by the use of the other CID functionalities after that join.  

When using it, you can optionally fill in the fields with the credentials of a domain administrator for what the CID try remotely delete the computer account from the AD database. If this fails, the operation of this function will not be affected.

>**Note:** A copy of the files modified during this process is stored in the `/var/lib/cid/backups/mod` directory.

#### Change station behavior <a name="behavior" />
This function allows you to change the options of the [Advanced mode](#advanced) after the system joins an AD domain.

>**Note:** Whenever a change is made through this função, a copy of the affected files in the state before modification is stored in the `/var/lib/cid/backups/mod` directory.

#### Block logon <a name="block" />
This function restricts logon in the system to a specific user or group of the domain. When selecting it you must inform the **Account Type** (User or Group) and the **Account Name**. If no type is selected, the _User_ type is assumed by default.

#### Unblock logon <a name="unblock" />
This function removes the logon restriction applied by the [block logon](#block) function.

#### Manage domain accounts in local groups <a name="account" />
This function allows you to associate domain user accounts with groups on the local system so that they can perform specific routines that require the administrative privileges of those groups, and it also allows them to execute the `sudo` command.  

Authentication for the sudo command can be enabled or disabled via the `Enable authentication for "sudo"` option in [advanced join mode](#advanced) or in [Change station behavior](#behavior).  
 
The CID uses the groups that the **default user** (usually the first user created on the system) belongs to to determine these groups. You can specify any other user using the `defaultuserid` parameter, and you can also add other specific groups using the `localgroups` parameter in the [cid.conf](#cid.conf) file.  

In the **Add account** option, you must inform the type and name of the account in the same way as when using the [block logon](#block) function.  

Note that on Unix systems there is no group nesting. Therefore, when a group account is added, members of that group are individually associated with local groups as they log on to this system. If you enter an asterisk (*) at the end of the group's name, all its members will be immediately associated. If the user is removed from the domain group, the user will be automatically removed from local groups on a next login to this computer.

The **Remove account** option lists domain accounts added to local groups and allows you to select them for removal.

>**Note:** Members of the **domain admins** group are already associated with local groups by default.

#### Manage shares <a name="shares" />
This function allows you to manage _Samba_ shares intuitively.  

The **Add share** option displays a form where you must enter the arguments to create or update a share.  

Three share modes are available:

##### Common mode <a name="common" />
This mode allows you to share a directory on one of the local file systems via Samba (_SMB protocol_).  

The directory path to be shared must be entered in the `Path` argument. If the directory path does not start with a forward slash (**/**) and you are using a `template` for the share, the parent directory of the template share is used as the parent directory for this share. If the directory does not exist, it will be created automatically.  

The access permissions of the share can be managed locally through the `Rule` argument, or through a remote Windows system using the **Microsoft Management Console** (MMC).  

When set locally, permissions are translated into extended _POSIX ACLs_ and interpreted by Samba as _Windows ACLs_. They must be composed of 03 fields separated by colons (:) and have the format **[Type]:Account:[Permission]**, where:

- **Type** is **u** for user or **g** for group;
- **Account** is a domain user or group;
- **Permission** is **r** for read-only, **f** for full access or **d** for access denied;

Other important aspects about permissions are:

- You can specify more than one permission at the same time, separating them with a semicolon (**;**).
- Use a plus sign (**+**) at the beginning of the expression to append a new permission to the pre-existing permissions of the shared directory.
- Use a minus sign (**-**) at the beginning of the expression to remove pre-existing permissions on the shared directory. In this case, the expression must contain only the type and name of the account (eg **-u:username**).
- The **everyone** term can be used in the **Account** field to represent **all users**.
- If no permissions are specified when creating a share, all users will be given **read-only** permission.

##### Userfolder mode <a name="userfolder" />
This mode enables the **homes** section of Samba, which is a special type of file sharing that automatically provides a share with the same name as the user who accesses it. In this mode, the `Path` argument must contain the path of the parent directory where the folders for each user share are to be created. If this argument is omitted, the `/home` directory will be assumed by default. If **disk quota** is used, it will be automatically applied to each user directory.

##### Printer mode <a name="printer" />
Mode used to share a printer configured on the local _CUPS_ server through Samba (_SMB protocol_). This is optional or unnecessary when the `Share all printers on CUPS` option (see [Advanced mode](#advanced)) is enabled. In this mode, the `Path` argument should be named after a printer configured in CUPS instead of a directory path. If left blank, this argument will receive the value used in the `Name` argument or vice versa. When printer sharing is enabled, a special file share called **print$** will be configured to automatically provide shared printer drivers to Windows clients on the network. The location of the directory to be used for this share can be adjusted using the `prtdrvdir` parameter in the [cid.conf](#cid.conf) file. Driver files must be manually copied to this directory or you must use a Windows system utility to manage the drivers on that print server (visit this <a href="https://wiki.samba.org/index.php/ Setting_up_Automatic_Printer_Driver_Downloads_for_Windows_Clients">link</a> for more information).  

Here is the complete list of arguments for a share:

| Argument                                | Description |
| --------------------------------------- | ----------- |
| **Mode**                                | Share mode. [Common mode](#common) is adopted if no selection is made. |
| **Name**                                | Name of a new share or share to be updated. In an update it is only necessary to fill in the fields that will be changed. |
| **Template**                            | Select a template for the share. The properties of the template will be copied to the share, except those that are specified. The Template argument can also be used to select a share to update, if the `Name` argument is left blank. [Userfolder mode](#userfolder) does not support this argument. |
| **Path**                                | Directory path or CUPS printer name. |
| **Rule**                                | Use this argument to manage share permissions in [Common mode](#common). |
| **Comment**                             | Provides a description for the share. This is optional. |
| **Disk Quota Size**                     | Disk quota applied to the shared directory in the [Common](#common) and [Userfolder](#userfolder) share modes. Use the letter **k**, **m**, **g**, **t**, **p**, **e**, **z** or **y** after an **integer** to indicate a unit (kilobytes, megabytes, gigabytes ...). If the unit is omitted, **k (kilobytes)** is adopted by default. Use the **zero (0)** number in this field to remove a disk quota previously applied to the share. The quota feature is currently only available for shared directories on **XFS** file systems. |
| **Tolerance Quota Size**                | Size of the tolerance quota that will be temporarily allowed when the disk quota is reached. The tolerance quota is optional and can only be used during the period determined by the `qttltime` parameter in [cid.conf](#cid.conf). After this period, the shared directory will not be able to write any more files until the use of its disk space is reduced to a value lower than the one defined in the disk quota size. Use the letter **k**, **m**, **g**, **t**, **p**, **e**, **z** or **y** after an **integer** to indicate a unit (kilobytes, megabytes, gigabytes...). If the unit is omitted, **k (kilobytes)** is adopted by default. Use the **zero (0)** number in this field to remove a tolerance quota previously applied to the share. The quota feature is currently available only for directories shared on **XFS** file systems. |
| **Apply Quota to Fst-level of Subdirs** | In [Common mode](#common) it applies the `disk quota` and `tolerance quota` (if defined) to the first level subdirectories instead of the shared directory. This is the default behavior for [Userfolder mode](#userfolder). |
| **Hidden**                              | If set to **Yes**, hides the share of the list of available shares in the net view and browse list. The default value is **No**. |
| **Allow Guest**                         | If set to **Yes**, allows the share to be accessed without authentication (no password) by any user. The default value is **No**. |
| **Add Config File**                     | It allows adding a file containing parameters to be attached to the settings made by the CID in the section of the share in the Samba configuration file (_smb.conf_). The contents of this file will be filtered so that there is no conflict with the parameters defined by default. |

The **Remove share** option displays the shares added by the tool and allows you to select them for deletion.  

When you delete a share from [Common mode](#common), _Extended POSIX ACLs_ are reset recursively on the file system. The user directories created by the [Userfolder](#userfolder) share mode, on the other hand, will not have their properties changed.  

When you delete a share of the [Printer mode](#printer), only the configuration in Samba is removed. The configuration of the printer on the CUPS server is not affected.

#### Help <a name="help" />
This function provides the following support options of the program:

| Option           | Description |
| ---------------- | ----------- |
| **About CID**    | Displays general information about the program. |
| **Station info** | This option displays the status of the settings made on the system after join a domain. |
| **App log**      | This displays the log generated by the program's functions on the **current day**. The complete log can be consulted in `/var/log/cid/functions.log`. |
| **Scripts log**  | This displays the log generated by the [logon scripts](#Logon_scripts) on the **current day**. The complete log can be consulted in `/var/log/cid/scripts.log`. |

>**Note:** The rows number of the log files can be adjusted by the `logsize` parameter in [cid.conf](#cid.conf) file.

### cid <a name="cid" />
The **cid** is the _CLI_ alternative to cid-gtk, and can be used to run all the features of the graphical tool on the command line or in bash scripts.

>**Note:** Use `man cid` command to access the complete manual for that utility.  

### cid-change-pass and cid-change-pass-gtk <a name="ccp_ccp-gtk" />
The `cid-change-pass` is a command line utility that allows domain users to change their passwords directly from the Linux computer joined to AD.  

The command must be executed by the user himself who wants to change his password. It must inform the program of its **current password** and the **new password**. This may contain restrictions according to the domain's security policies.  

If run as a superuser (**root**), the program should receive one or more domain user accounts as an argument. You will need to enter the current password for each of these users to perform the password change procedure.

The `cid-change-pass-gtk` tool provides a simple and intuitive graphical interface to the `cid-change-pass` utility.

### cid.conf <a name="cid.conf" />
The **cid.conf** is a configuration file installed in the `/etc/cid` directory which allows you to adjust the values of the variables used in the CID scripts adapting its operation to the Linux environment to which it was installed.  

The parameters in this file are declared as bash variables in the following format:

	parameter=value

Note that there should be no spaces between the **equal sign (=)** that separates the parameter from its corresponding value. Some parameters admit more than one value, and in these cases, the values must be separated by a **single space**, but between **single quotes (')** or **double quotes (")**, which are mandatory if _environment variables_ are used in the value of a parameter.

Lines that start with a **hashtag (#)** are just comments and are not interpreted by the program. The file of the original package contains all accepted parameters accompanied by comments that describe them widely. You can access a copy of this file at this <a href="https://sourceforge.net/projects/c-i-d/files/docs/cid.conf">link</a>.

>**Note:** Modifications made to the file after the system has already been joined to a domain will only be applied if you reload the [CID Init Script](#CIS) with the command: `systemctl reload cid`.

### CID Init Script <a name="CIS" />
The **CID Init Script** is a _Unit file_ for _Systemd_ generated when the computer joins the domain and is responsible for maintaining the settings made by the CID in the system files, and also for making other adjustments during startup, such as forcing time synchronization with the domain controllers and update the `/etc/hosts` file (useful in DHCP-based networks).  

You can manually force reconfiguration of files modified by the CID using the command: `systemctl reload cid`. This will also cause the changes made to the [cid.conf](#cid.conf) file after the system joins the domain to be applied.  

The CID Init Script is a unit file of the _oneshot_ type and does not keep active processes as a common service after the system initialization.

>**Note:** When removing the computer from the domain the CID Init Script is also removed from the system.

## Logon scripts <a name="Logon_scripts" />
The CID can configure the Linux system to allow bash scripts stored in the **Netlogon** share to be executed when the domain users will made logon.  

Basically the process consists of mounting the Netlogon on local file system, and then executed the scripts in the system shell. In the end, Netlogon is unmounted from the file system again. All of this is done automatically and transparently to the user.

If the `Disable logon scripts` option is not enabled (see [Advanced mode](#advanced)), during the system [joining the domain](#join), and if it does not already exist, the CID tries to export to Netlogon a default copy of the **scripts_cid** directory using the credentials of the domain administrator user used to the join. This copy is provided with the program's source package and is installed in the `/usr/share/cid/templates` directory. This directory contains two bash scripts ([logon.sh and logon_root.sh](#logon_lroot.sh)) and the [shares.xml](#map_shares) file. See the details about these files in the next subsections!

### logon.sh and logon_root.sh <a name="logon_lroot.sh" />
The **logon_root.sh** is the bash script executed with the **root user UID**, which grants privileges for the execution of routines that require the powers of the superuser. It is executed **before** logon.sh.  

The **logon.sh** is the bash script executed with the UID of the user who is logging in and can be used to execute routines that do not require elevated privileges. It is executed **after** logon_root.sh.  

In both scripts, in addition to the known bash variables, you can use the following environment variables exported by the CID:

| Variable        | Description |
| --------------- | ----------- |
| **NETLOGON**    | Mount point of the Netlogon share on the system. |
| **USERNAME**    | Name of the user that are opening session. |
| **USERID**      | ID of the user that are opening session. |
| **USERDOMAIN**  | Domain name of the user that is opening session. |
| **USERPROFILE** | Home directory path of the user that is opening session. |
| **USERSHELL**   | Login shell of the user that is opening session. |
| **GROUPID**     | Primary group ID of the user that is opening session. |
| **USERGROUP**   | Primary group name of the user that is opening session. |
| **USERGROUPS**  | Group list separated by commas (**,**) of the user that is opening session. |

### Automatic mapping of file shares <a name="map_shares" />
The automatic mapping of file shares during users logon can be performed through the **pam_mount** module.  

<a href="http://pam-mount.sourceforge.net/">Pam_mount</a> is a library that allows you to mount a file system transparently through PAM authentication. In general, the file systems to be mounted are defined in your global configuration file (usually stored in `/etc/security/pam_mount.conf.xml`). The **shares.xml** file is nothing more than a copy of this file stored on Netlogon, which replaces the original configuration file on Linux computers of the domain during users logon. In this way, all configuration can be centralized in this single file to be applied to all theses computers.  

The file systems to be mounted are defined in `<volume ...>` tags. The default shares.xml file contains some examples of defining common SMB shares, such as:

	<!-- Mapping a public share (full access to everyone) -->
	<volume fstype="cifs" server="fileserver" path="PUBLIC" mountpoint="~/PUBLIC" />

	<!-- Mapping the user folder (a "homes" section created by CID on Samba) -->
	<volume fstype="cifs" server="fileserver" path="%(USER)" mountpoint="~/MyNetFolder" />

	<!-- Conditional mapping to the ITD group domain -->
	<volume sgrp="itd" fstype="cifs" server="fileserver" path="ITD$" mountpoint="~/ITD" />

	<!-- Conditional mapping to a ITD group of other trusted domain -->
	<volume sgrp="SAMPLE\itd" fstype="cifs" server="fileserver" path="ITD$" mountpoint="~/ITD" options="domain=SAMPLE" />

See the <a href="http://pam-mount.sourceforge.net/pam_mount.conf.5.html">pam_mount documentation</a> for more information on its configuration.

### Automatic mapping of shared printers <a name="map_printers" />
The basis for automating the mapping of printers is to use the **lpadmin** command in the [logon scripts](#Logon_scripts).  

<a href="https://man7.org/linux/man-pages/man8/lpadmin.8.html">Lpadmin</a> is a command line utility that configures printers or print classes for _CUPS_. With it, you can easily add, remove or even set a printer as a default on the Linux system.  

Generally, printer management is only allowed for the root user or users who are members of the CUPS management group, whose name may vary from one Linux distribution to another. For this reason, settings for automatic mapping should normally be made in the [logon_root.sh](#logon_lroot.sh) script.

	# Eg: Mapping printer-01 only to the administrator user
	[[ "$USERNAME" == "administrator" ]] && lpadmin -p printer-01 -E -v ipp://printserver/ipp/printer-01 -m everywhere

## Troubleshooting <a name="Troubleshooting" />
This section describes known cases related to the program.

### Hostname longer than 15 characters <a name="Hostname" />
Due to a limitation inherited from the NetBIOS API, hostnames cannot contain more than 15 characters (more information <a href="https://support.microsoft.com/en-us/kb/909264">here</a>). If necessary, adjust the hostname of the Linux computer before attempting to join the domain.

### Graphic Mode Login Managers <a name="Login" />
In certain Linux distributions, some login managers in their default configuration do not list users who are in a remote source and/or do not have options so that the credentials of those users can be informed through a prompt or text box. In such cases you should consult the manual of your distribution or of the login manager in question to see if these options are available and what settings should be made. If necessary, install a new graphical login manager.

<br>

>Release 1.1.2 2021-05-15  
>
>*Copyright (C) 2012-2021 Eduardo Moraes <<emoraes25@gmail.com>>*
