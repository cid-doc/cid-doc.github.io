[CID](https://cid-doc.github.io/)
===

**License: [GPLv3](https://www.gnu.org/licenses/gpl-3.0.html)**  

[Homepage](https://c-i-d.sourceforge.io) | [Discussion](https://sourceforge.net/p/c-i-d/discussion/) | [Donations](https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=UA5SSGEKPUXPQ&lc=US&item_name=Closed%20In%20Directory&currency_code=USD&bn=PP%2dDonationsBF%3abtn_donateCC_LG%2egif%3aNonHosted) | [Changelog](docs/CHANGELOG.md)  


## Contents
- [Description](#Description)
- [Requirements](#Requirements)
- [Installation](#Installation)
    - [Ubuntu](#Ubuntu)
    - [Debian](#Debian)
    - [Fedora](#Fedora)
    - [OpenSUSE](#OpenSUSE)
    - [Other distros](#Other)
- [cid-gtk](#cid-gtk)
    - [Join the domain](#join)
        - [Advanced mode](#advanced)
    - [Remove from domain](#leave)
    - [Change station behavior](#behavior)
    - [Block logon](#block)
    - [Unblock logon](#unblock)
    - [Manage AD accounts in local groups](#account)
    - [Manage shares](#shares)
        - [Common mode](#common)
        - [Userfolder mode](#userfolder)
        - [Printer mode](#printer)
    - [Help](#help)
- [cid](#cid)
- [cid-change-pass and cid-change-pass-gtk](#ccp_ccp-gtk)
- [cid.conf](#cid.conf)
- [CID Init Script](#CIS)
- [Logon scripts](#Logon_scripts)
    - [logon.sh and logon_root.sh](#logon_lroot.sh)
    - [Automatic mounting of shared folders](#map_shares)
    - [Automatic configuration of printers](#map_printers)
- [Troubleshooting](#Troubleshooting)
    - [Graphic Mode Login Managers](#Login)
    - [Failed to join machines to .local domains](#.local)
    - [Numeric users login failed](#numericuser)

## Description <a name="Description" />
**CID** (*Closed In Directory*) is a set of bash scripts for inserting and managing Linux computers in <a href="https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview">Active Directory (AD)</a> domains. Modifications made to the system allow Linux to behave like a Windows computer within AD.  

You can do things like:  
- Run logon scripts
- Automatic mounting of network drives (shared folders)
- Automatic configuration of the printers
- Offline logon
- Automatic granting of privileges to users (e.g. access to the `sudo` command)
- GUI and CLI tools for managing the <a href="https://wiki.samba.org/index.php/Main_Page">Samba</a> server (File and Print Server)
- Apply disk quota per shared directory (such as Windows Server)

CID consists of four main tools subdivided into two GUI tools ([cid-gtk](#cid-gtk) and [cid-change-pass-gtk](#ccp_ccp-gtk)) and two CLI utilities ([cid](#cid) and [cid-change-pass](#ccp_ccp-gtk)). Both pairs contain equivalent features and they all accept the following general options as a argument in the command line:  

| Options           | Description      |
| ----------------- | ---------------- |
| **-v, --version** | Show the version |
| **-h, --help**    | Show the help    |


## Requirements <a name="Requirements" />
- acl (= any)
- attr (= any)
- awk (= any)
- bash (>= 4)
- cifs-utils (>= 6.4)
- CUPS (= any)
- {diff,find,core}utils (= any)
- grep (= any)
- gzip (= any)
- hostname (= any)
- iproute[2] (= any)
- Kerberos (>= 1.13)
- keyutils (= any)
- mount (= any)
- pam_mount (>= 2.14)
- passwd (= any)
- ping (= any)
- pkexec (= any)
- Samba (>= 4.3.11)
- sed (= any)
- sudo (= any)
- Systemd (= any)
- Winbind (>= 4.3.11)
- xhost (= any)
- zenity (>= 3.18.1)


## Installation <a name="Installation" />
### Ubuntu <a name="Ubuntu" />

    $ sudo add-apt-repository -y ppa:emoraes25/cid
    $ sudo apt update && sudo apt -y install cid cid-gtk

### Debian <a name="Debian" />

    $ keys_dir='/etc/apt/keyrings' ; repos_dir="${keys_dir%/*}/sources.list.d"
    $ [ -d "$keys_dir" ] || sudo mkdir -m0755 -p "$keys_dir"
    $ wget -qO - https://cid-doc.github.io/keys/CID-GPG-KEY | sudo gpg --dearmor -o "${keys_dir}/cid-archive-keyring.pgp"
    $ sudo wget -qO "${repos_dir}/cid.sources" https://cid-doc.github.io/docs/cid.sources
    $ sudo apt update && sudo apt -y install cid cid-gtk

### Fedora <a name="Fedora" />

    $ sudo rpm --import https://cid-doc.github.io/keys/CID-GPG-KEY
    $ sudo dnf config-manager addrepo --from-repofile=https://cid-doc.github.io/docs/cid.repo
    $ sudo dnf install cid

### OpenSUSE <a name="OpenSUSE" />

    $ sudo rpm --import https://cid-doc.github.io/keys/CID-GPG-KEY
    $ sudo zypper ar https://cid-doc.github.io/docs/cid.repo
    $ sudo zypper in cid

### Other distros <a name="Other" />
After installing the [requirements](#Requirements), download the tarball, unzip it and run as **root** the **INSTALL.sh** script. You can use the following commands:

    $ ver='x.x.x' #current version
    $ wget https://downloads.sf.net/c-i-d/files/cid-${ver}.tar.gz    
    $ tar -xzf cid-${ver}.tar.gz
    $ cd cid-${ver}
    $ sudo ./INSTALL.sh

>**Note:** Run `sudo ./INSTALL.sh uninstall` to uninstall the program files from the same version of the package.


## cid-gtk <a name="cid-gtk" />
The **cid-gtk** is the tool that contains the main features of the program. Through it you can insert your Linux computer in an AD domain and later manage a series of functions in the system.  

The available features are described in the following sections.

### Join the domain <a name="join" />
This function allows you to join the Linux computer to an AD domain. For that, it is necessary to inform the domain data in the respective fields as shown in the table below:

| Field                   | Description |
| ----------------------- | ----------- |
| **Domain**              | Domain name (FQDN). |
| **Hostname** | Name for computer account that will be created in AD. If not specified, the account will be created with the same hostname defined in the system. |
| **Organizational Unit** | Optionally, you can specify an Organizational Unit where the computer account will must be created when join it the domain. If the OU is not entered or is not found, the computer account will be created in the default container (computers). |
| **User**                | Domain administrator user. |
| **Password**            | User password. |
| **Mode**                | Select one of two join modes: **Default** or **Advanced**. _Default_ mode is adopted if no selection is made. [Advanced mode](#advanced) opens a form that allows you to customize the settings that the CID will perform on the system during the process of joining the domain. All configuration options available in this mode are directly opposite to the settings adopted in the Default mode. |

>**Note:** Before modifying the system files, the CID makes a backup in the `/var/lib/cid/backups/ori` directory.

#### Advanced mode <a name="advanced" />
The options available in this mode are:

| Option                                  | Description |
| --------------------------------------- | ----------- |
| **Disable NetBIOS over TCP/IP**         | Disables support for the <a href="https://en.wikipedia.org/wiki/NetBIOS">NetBIOS API</a> implemented by Samba. |
| **Disable authentication via Kerberos** | This causes the <a href="https://www.samba.org/samba/docs/current/man-html/pam_winbind.8.html">pam_winbind</a> module to not attempt to obtain the <a href="https://web.mit.edu/kerberos">kerberos</a> tickets known as Ticket Granting Tickets (TGTs) of the Authentication Server (AS) during user login. |
| **Disable credential caching**          | Disables support for off-line authentication, or authentication with local cached credentials. This requires real-time communication with the authentication server for the logins the domain users. |
| **Disable logon scripts**               | This disables [logon scripts](#Logon_scripts). |
| **Do not use domain as default**        | Configures <a href="https://www.samba.org/samba/docs/current/man-html/winbindd.8.html">winbind</a> not to use the joined domain as the system default. This makes it necessary to specify the domain name before the user or group name (format: `DOMAIN\user` or `DOMAIN\group`), both in authentication and in system commands that receive user or group names as argument. |
| **Enable authentication for sudo**    | This requires that domain users who are given administrative privileges on the Linux computer need to authenticate when running the `sudo` command (see [Manage AD accounts in local groups](#account)). |
| **Use idmap_ad (RFC 2307)**           | This option allows the use of the <a href="https://www.samba.org/samba/docs/current/man-html/idmap_ad.8.html">idmap_ad</a> backend, which implements an API to obtain the Unix attributes of users and groups in the domain through domain controllers, as long as they have NIS extensions enabled. By default, the CID configures winbind to use the <a href="https://www.samba.org/samba/docs/current/man-html/idmap_autorid.8.html">idmap_autorid</a> backend, which establishes these attributes through a predefined configuration on the local system. You can use the `id_range_size`, `max_num_domains`, `wbd_userprofile` and `wbd_usershell` parameters in [cid.conf](#cid.conf) to customize this configuration. When selecting this option, a new form will be presented for configuring the backend with the following fields:<br /><br />- **Initial ID:** Initial value of the range of **UIDs** and **GIDs** that will be mapped by the backend. This field is required, and the value assigned must be greater than the IDs already used by local users and groups;<br /><br />- **Final ID:** End value of the range of **UIDs** and **GIDs** that will be mapped by the backend. When not set, the CID will use a random value based on the value set in the Initial ID field;<br /><br />- **winbind nss info:** This defines whether information about the **home directory** and the **shell** of domain users should also be obtained by the DC with the `rfc2307` option, or whether through a predefined configuration with the `template` option. The **template** option is adopted by default. |
| **Share all printers on CUPS**          | Enables automatic sharing of all printers configured on the local <a href="http://www.cups.org/">CUPS</a> server through Samba (_SMB protocol_). This makes it unnecessary to configure individual shares for each printer through the [Manage shares](#shares) option. |
| **Use keytab file method**              | Configures Samba to use a dedicated <a href="http://web.mit.edu/kerberos/krb5-1.14/doc/basic/keytab_def.html">keytab</a> file as an <a href="https://web.mit.edu/kerberos">kerberos</a> authentication method. The `krb_principal_names` parameter in the [cid.conf](#cid.conf) file can be used to specify principal names that you want to be added to the keytab. |
| **Add config file to Samba**          | It allows adding a file containing parameters to be attached to the _Global_ section of the samba configuration file (_smb.conf_). The contents of this file will be filtered so that there is no conflict with the parameters predefined by the CID. |

### Remove from domain <a name="leave" />
This function undoes the modifications made in the system for the computer to [join the domain](#join), and by the use of the other CID functionalities after that join.  

When using it, you can optionally fill in the fields with the credentials of a domain administrator for what the CID try remotely delete the computer account from the AD database. If this fails, the operation of this function will not be affected.

>**Note:** A copy of the files modified during this process is stored in the `/var/lib/cid/backups/mod` directory.

### Change station behavior <a name="behavior" />
This function allows you to change the options of the [Advanced mode](#advanced) after the system joins an AD domain.

>**Note:** Whenever a change is made through this função, a copy of the affected files in the state before modification is stored in the `/var/lib/cid/backups/mod` directory.

### Block logon <a name="block" />
This function restricts logon in the system to a specific user or group of the domain. When selecting it you must inform the **Account Type** (User or Group) and the **Account Name**. If no type is selected, the _User_ type is assumed by default.

### Unblock logon <a name="unblock" />
This function removes the logon restriction applied by the [block logon](#block) function.

### Manage AD accounts in local groups <a name="account" />
This function allows you to associate AD user accounts with groups on the local system so that they can perform specific routines that require the administrative privileges of those groups, in addition to allowing them to run the `sudo` command.  

Authentication for the sudo command can be enabled or disabled via the `Enable authentication for "sudo"` option in [advanced join mode](#advanced) or in [Change station behavior](#behavior).  
 
The CID uses the groups that the **default user** (usually the first user created on the system) belongs to to determine these groups. You can specify any other user using the `defaultuserid` parameter, and you can also add other specific groups using the `localgroups` parameter in the [cid.conf](#cid.conf) file.  

In the **Add account** option, you must inform the **type** and **name** of the account in the same way as when using the [block logon](#block) function.  

Note that on Unix systems there is no group nesting. Therefore, when a group account is added, members of that group are individually associated to the local groups as they log on to this system. If you enter an asterisk (*) at the end of the group's name, all its members will be immediately associated. If the user is removed from the domain group, the user will be automatically removed from local groups on a next login to this computer.

The **Remove account** option lists domain accounts added to local groups and allows you to select them for removal.

>**Note:** Members of the **Domain Admins** group are automatically associated with local groups.

### Manage shares <a name="shares" />
This function allows you to manage _Samba_ shares intuitively.  

The **Add share** option displays a form where you must enter the arguments to create or update a share.  

Three share modes are available:

#### Common mode <a name="common" />
This mode allows you to share a directory on one of the local file systems via Samba (_SMB protocol_).  

The directory path to be shared must be entered in the `Path` argument. If the directory path does not start with a forward slash (**/**) and you are using a `template` for the share, the parent directory of the template share is used as the parent directory for this share. If the directory does not exist, it will be created automatically.  

The access permissions of the share can be managed locally through the `Rule` argument, or through a remote Windows system using the <a href="https://docs.microsoft.com/en-us/troubleshoot/windows-server/system-management-components/what-is-microsoft-management-console">Microsoft Management Console (MMC)</a>.  

When set locally, permissions are translated into <a href="https://www.usenix.org/legacy/publications/library/proceedings/usenix03/tech/freenix03/full_papers/gruenbacher/gruenbacher_html/main.html">extended POSIX ACLs</a> and interpreted by Samba as <a href="https://docs.microsoft.com/en-us/windows/win32/secauthz/access-control-lists">Windows ACLs</a>. They must be composed of 03 fields separated by colons (:) and have the format **[Type]:Account:[Permission]**, where:

- **Type** is **u** for user or **g** for group;
- **Account** is a domain user or group;
- **Permission** is **r** for read-only, **f** for full access or **d** for access denied;

Other important aspects about permissions are:

- You can specify more than one permission at the same time, separating them with a semicolon (**;**).
- Use a plus sign (**+**) at the beginning of the expression to append a new permission to the pre-existing permissions of the shared directory.
- Use a minus sign (**-**) at the beginning of the expression to remove pre-existing permissions on the shared directory. In this case, the expression must contain only the type and name of the account (eg **-u:username**).
- The `everyone` term can be used in the **Account** field to represent **all users**.
- If no permissions are specified when creating a share, **all users** will be given **read-only** permission.

Some examples:

###### Allowing full access to the Domain Users group
	g:domain users:f

###### Adding read-only permission to the Guest user account
	+u:guest:r

###### Removing permissions for the Guest user account
	-u:guest

###### Allowing full access to all users and denying access to the Guests group
	u:everyone:f;g:guests:d

#### Userfolder mode <a name="userfolder" />
This mode enables the **homes** section of Samba, which is a special type of file sharing that automatically provides a share with the same name as the user who accesses it. In this mode, the `Path` argument must contain the path of the parent directory where the diretories for each user are to be created. If this argument is omitted, the `/home` directory will be assumed by default. CID automatically creates user directories with appropriate permissions as users first access their respective shares. If **disk quota** is used, it will be automatically applied to each user directory.

#### Printer mode <a name="printer" />
Mode used to share a printer configured on the local _CUPS_ server through Samba (_SMB protocol_). This is optional or unnecessary when the `Share all printers on CUPS` option (see [Advanced mode](#advanced)) is enabled. In this mode, the `Path` argument must receive the name of a configured printer in CUPS instead of a directory path. If left blank, this argument will receive the value used in the `Name` argument or vice versa. When printer sharing is enabled, a special file share called **print$** will be configured to automatically provide shared printer drivers to Windows clients on the network. The location of the directory to be used for this share can be adjusted using the `prtdrvdir` parameter in the [cid.conf](#cid.conf) file. Driver files must be manually copied to this directory or you must use a Windows system utility to manage the drivers on that print server (visit this <a href="https://wiki.samba.org/index.php/ Setting_up_Automatic_Printer_Driver_Downloads_for_Windows_Clients">link</a> for more information).  

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
| **Tolerance Quota Size**                | Size of the tolerance quota that will be temporarily allowed when the disk quota is reached. The tolerance quota is optional and can only be used during the period determined by the `qttltime` parameter in [cid.conf](#cid.conf). After this period, the shared directory will not be able to write any more files until the use of its disk space is reduced to a value lower than the one defined in the `Disk Quota Size`. Use the letter **k**, **m**, **g**, **t**, **p**, **e**, **z** or **y** after an **integer** to indicate a unit (kilobytes, megabytes, gigabytes...). If the unit is omitted, **k (kilobytes)** is adopted by default. Use the **zero (0)** number in this field to remove a tolerance quota previously applied to the share. The quota feature is currently available only for directories shared on **XFS** file systems. |
| **Apply Quota to Fst-level of Subdirs** | In [Common mode](#common) it applies the `disk quota` and `tolerance quota` (if defined) to the first level subdirectories instead of the shared directory. This is the default behavior for [Userfolder mode](#userfolder). |
| **Hidden**                              | If set to **Yes**, hides the share of the list of available shares in the net view and browse list. The default value is **No**. |
| **Allow Guest**                         | If set to **Yes**, allows the share to be accessed without authentication (no password) by any user. The default value is **No**. |
| **Add Config File**                     | It allows adding a file containing parameters to be attached to the section of the share in the Samba configuration file (_smb.conf_). The contents of this file will be filtered so that there is no conflict with the parameters predefined by CID. |

The **Remove share** option displays the shares added by the tool and allows you to select them for deletion.  

When you delete a share from [Common mode](#common), _Extended POSIX ACLs_ are reset recursively on the file system. The user directories created by the [Userfolder](#userfolder) share mode, on the other hand, will not have their properties changed.  

When you delete a share of the [Printer mode](#printer), only the configuration in Samba is removed. The configuration of the printer on the CUPS server is not affected.

### Help <a name="help" />
This function provides the following support options of the program:

| Option           | Description |
| ---------------- | ----------- |
| **About CID**    | Displays general information about the program. |
| **Station info** | This option displays the status of the settings made on the system after join a domain. |
| **App log**      | This displays the log generated by the program's functions on the **current day**. The complete log can be consulted in `/var/log/cid/functions.log`. |
| **Scripts log**  | This displays the log generated by the [logon scripts](#Logon_scripts) on the **current day**. The complete log can be consulted in `/var/log/cid/scripts.log`. |

>**Note:** The rows number of the log files can be adjusted by the `logsize` parameter in [cid.conf](#cid.conf) file.

## cid <a name="cid" />
The **cid** is the _CLI_ alternative to [cid-gtk](#cid-gtk), and can be used to run all the features of the graphical tool on the command line or in bash scripts.

>**Note:** Use `man cid` command to access the complete manual for that utility.  

## cid-change-pass and cid-change-pass-gtk <a name="ccp_ccp-gtk" />
The `cid-change-pass` is a command line utility that allows domain users to change their passwords directly from the Linux computer joined to AD.  

The command must be executed by the user himself who wants to change his password. It must inform the program of its **current password** and the **new password**. This may contain restrictions according to the domain's security policies.  

If run as a superuser (**root**), the program should receive one or more domain user accounts as an argument. You will need to enter the current password for each of these users to perform the password change procedure.

The `cid-change-pass-gtk` tool provides a simple and intuitive graphical interface to the `cid-change-pass` utility.

## cid.conf <a name="cid.conf" />
The **cid.conf** is a configuration file installed in the `/etc/cid` directory which allows you to adjust the values of the variables used in the CID scripts adapting its operation to the Linux environment to which it was installed.  

The parameters in this file are declared as bash variables in the following format:

	parameter=value

Note that there should be no spaces between the **equal sign (=)** that separates the parameter from its corresponding value. Some parameters admit more than one value, and in these cases, the values must be separated by a **single space**, but between **single quotes (')** or **double quotes (")**, which are mandatory if _environment variables_ are used in the value of a parameter.

Lines that start with a **hashtag (#)** are just comments and are not interpreted by the program. The original file of the package contains all accepted parameters accompanied by comments that describe them widely. You can access a copy of this file at this <a href="docs/cid.conf.example">link</a>.

>**Note:** Modifications made to the file after the system has already been joined to a domain will only be applied if you reload the [CID Init Script](#CIS) with the command: `systemctl reload cid`.

## CID Init Script <a name="CIS" />
The **CID Init Script** is a _Unit file_ for _Systemd_ generated when the computer joins the domain and is responsible for maintaining the settings made by the CID and also for making other adjustments during system initialization, such as forcing time synchronization with the domain controllers and update the `/etc/hosts` file (useful in DHCP-based networks).  

You can manually force reconfiguration of files modified by the CID using the command: `systemctl reload cid`. This will also cause the changes made to the [cid.conf](#cid.conf) file after the system joins the domain to will be applied.  

The CID Init Script is a unit file of the **oneshot** type and does not keep active processes as a common service after the system initialization.

>**Note:** When removing the computer from the domain the CID Init Script is also removed.

## Logon scripts <a name="Logon_scripts" />
The CID can configure the Linux computer to allow bash scripts stored on the **Netlogon** share to run when domain users logon.  

Basically the process consists of mounting the Netlogon on local file system, and then executed the scripts in the system shell. In the end, Netlogon is unmounted again. All of this is done automatically and transparently to the user.

If the `Disable logon scripts` option is not enabled (see [Advanced mode](#advanced)), during the system [joining the domain](#join), and if it does not already exist, the CID tries to export to Netlogon a template of this scripts using the credentials of the domain administrator user used to the join. This files is provided with the program’s source package and is installed in the `/usr/share/cid/templates` directory. See the details about the scripts in the next subsections!

>**Note:** The scripts were stored in the **scripts_cid** folder in _Netlogon_. As of version **1.2.5** this folder has been renamed to **cid** only!

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

### Automatic mounting of shared folders <a name="map_shares" />
The automatic mounting of shared folders during users logon can be performed through the **pam_mount** module.  

<a href="http://pam-mount.sourceforge.net/">Pam_mount</a> is a library that allows you to mount a file system transparently through PAM authentication. In general, the file systems to be mounted are defined in your global configuration file (usually `/etc/security/pam_mount.conf.xml`). The **shares.xml** file is nothing more than a copy of this file stored on Netlogon, which replaces the original configuration file on Linux computers of the domain during users logon. In this way, all configuration can be centralized in this single file to be applied to all theses computers.  

The file systems to be mounted are defined in `<volume ...>` tags. The default shares.xml file contains some examples of defining common SMB shares, such as:

	<!-- Mounting the 'public' share -->
	<volume fstype="cifs" server="fileserver" path="public" mountpoint="~/public" />

	<!-- Mounting the user's network folder -->
	<volume fstype="cifs" server="fileserver" path="%(USER)" mountpoint="~/MyNetFolder" />

	<!-- Mounting the 'IT$' share for 'ITD' group users -->
	<volume sgrp="itd" fstype="cifs" server="fileserver" path="it$" mountpoint="~/IT" />

	<!-- Mounting the 'HR' share for users in the 'HRD' group of the 'SAMPLE' trusted domain -->
	<volume sgrp="SAMPLE\hrd" fstype="cifs" server="fileserver" path="hr$" mountpoint="~/HR" options="domain=SAMPLE" />

	<!-- Mounting the 'cameras' share for users in the 'security' or 'monitors' group of the 'example.com' domain -->
	<volume fstype="cifs" server="fileserver" path="cameras" mountpoint="~/cameras" options="domain=example.com">
		<or>
			<sgrp>security</sgrp>
			<sgrp>monitors</sgrp>
		</or>
	</volume>

See the <a href="http://pam-mount.sourceforge.net/pam_mount.conf.5.html">pam_mount documentation</a> for more information on its configuration.

### Automatic configuration of printers <a name="map_printers" />
One way to automate printers setup is to use the **lpadmin** utility in [logon scripts](#Logon_scripts).  

<a href="https://man7.org/linux/man-pages/man8/lpadmin.8.html">Lpadmin</a> is a command line utility that configures printers or print classes for _CUPS_. With it, you can easily add, remove or even set a printer as a default on the Linux system.  

Generally, printer management is only allowed for the root user or users who are members of the CUPS management group, whose name may vary from one Linux distribution to another. For this reason, definitions of printers mapping should normally be made in the [logon_root.sh](#logon_lroot.sh) script.

	# Eg: Adding printer to the system
	lpadmin -p printer -E -v ipp://printer_address/ipp/printer -m everywhere

## Troubleshooting <a name="Troubleshooting" />
This section describes known cases related to the program.

### Graphic Mode Login Managers <a name="Login" />
In certain Linux distributions, some login managers in their default configuration do not list users who are in a remote source and/or do not have options so that the credentials of those users can be informed through a prompt or text box. In such cases you should consult the manual of your distribution or of the login manager in question to see if these options are available and what settings should be made. If necessary, install a new graphical login manager.

### Failed to join machines to .local domains <a name=".local" />
It is common to get an error when joining the machine in domains with the <a href="https://en.wikipedia.org/wiki/.local">.local</a> top-level domain (TLD). This is because the .local domain was reserved for applications that use DNS Multicast (mDNS) to provide zero-configuration networks (zeroconf). On linux, <a href="https://en.wikipedia.org/wiki/Avahi_(software)">Avahi</a> is an example of these applications. Generally, mDNS resolution is prioritized over Unicast DNS resolution, which prevents querying by domain controllers (DCs) from reaching the domain's DNS servers. A way around this is to associate the IP address of one of the DCs with the domain name in the `/etc/hosts` file, for example:

	echo -e "192.168.25.25\texample.local" | sudo tee -a /etc/hosts

Being:

- **192.168.25.25** the IP of the DC;
- **example.local** the domain name;

### Numeric users login failed <a name="numericuser" />
Since the <a href="https://github.com/systemd/systemd">systemd</a> session manager stopped supporting user accounts whose login consists only of numbers (<a href="https://github.com/systemd/systemd/issues/15141">bug #15141</a>), it has been a problem for AD administrators to make it possible to log on of accounts with this format on Linux computers integrated into AD, either through CID or other similar tools. Fortunately, there is an unusual way to resolve this issue!

In the CID, you can enable the `Do not use domain as default` option among the join options of the **Advanced mode** or the **Change station behavior** option, if the computer is already in AD. You can also use the `--no-defaultdomain` option along with the `join` subcommand on the command line (eg: `cid join -p --no-defaultdomain`). This will cause user accounts to be identified on the system with the format **DOMAIN\username**, which ensures that there are non-numeric characters as well. The bad part is that this implies in the user must always enter the domain along with the username to log on, either in the UPN format (**username@fqdn**) or in the low-level login name format itself (**DOMAIN\username**).

<br>

>Release 1.2.6 2025-06-11  
>Copyright (C) 2012-2025 Eduardo Moraes <<emoraes25@gmail.com>>
