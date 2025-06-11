Release 1.2.5 2023-06-11
------------------------
- scripts_cid folder replaced with cid folder in Netlogon.
- Adjusted the **winbind request timeout** parameter in smb.conf
from 60s (default) to 5s for a better response to applications
contacting the *Winbindd* daemon in offline environments.

Release 1.2.4 2022-07-23
------------------------
- Added support to logon scripts for TDM and CDM login managers.

Release 1.2.3 2022-07-13
------------------------
- Fixed bug in Get_DomainIP function.
- Fixed bug in Mount_Netlogon function.

Release 1.2.2 2022-06-13
------------------------
- Added **Hostname** field to `Join the domain` option in cid-gtk.
- Added **host** argument to `join` subcommand in cid (CLI).
- Forces mount the *Netlogon* using old versions of the SMB
protocol (compatible with versions prior to *Windows Server 2012*).

Release 1.2.1 2021-12-09
------------------------
- Fixed bug creating working directories during installation on Fedora.
- Fixed display of IP addresses in `status` subcommand.
- Fixed error when leaving the domain in IPv6 networks.
- Removed user list hiding setting in Lightdm.

Release 1.2.0 2021-11-12
------------------------
- Fixed error showing file selection dialog after selecting
`Add config file to Samba` option in advanced join mode of cid-gtk.
- `Manage domain accounts in local groups` option has been replaced by
`Manage AD accounts in local groups` in cid-gtk.
- Optimization of source code.

Release 1.1.9 2021-11-10
------------------------
- Fixed overwrites of the cid.conf file in updates.
- Optimization of source code.

Release 1.1.8 2021-11-05
------------------------
- Fixed PAM configuration error.
- Fixed CID Init Script configuration error.

Release 1.1.7 2021-11-03
------------------------
- Fixed display of IP addresses in status.
- Fixed domain join failure on IPv6 networks.
- Fixed mount of netlogon on IPv6 networks.
- New PolicyKit policy file.
- Updated .desktop files.
- Updated bash completion script.
- Added commit requirement to perform `cid leave` with no arguments.
- Added filtering the `cid acccount list` command by account name.
- Added filtering the `cid share list` command by share name.
- New `excl_localgroups` parameter in cid.conf.
- Optimization of source code.

Release 1.1.6 2021-08-22
------------------------
- Changed CID logs to Syslog format.
- Disabled the debug flag of the pam_winbind module.

Release 1.1.5 2021-07-31
------------------------
- Code optimization.

Release 1.1.4 2021-07-27
------------------------
- Fixed testing of global IPv6 addresses.
- Changed installation of Bash completion script from the `/etc/bash_completion.d`
to `/usr/share/bash-completion/completions` directory.
- Added support for logon scripts on xrdp connections.

Release 1.1.3 2021-07-04
------------------------
- Fixed offline authentication failure in Debian/Ubuntu based distros.
- Activation of the Samba support to Group Policies (GPO) along with
the CID logon scripts.
- Updated documentation address.

Release 1.1.2 2021-05-15
------------------------
- New `add_pam_authmod` and `pam_mod_dir` parameters added to the cid.conf file.
- lsb-release removed from requirements.
- Fix of the change in behavior options by the cid utility.

Release 1.1.1 2021-03-19
------------------------
- Added support for IPv6 addresses.
- Masking the Samba NMB Daemon on Systemd when disabling Netbios over TCP/IP.
- Removed the Samba SMB and NMB daemons from the "Wants" and "After" list
in the CID Unit File.

Release 1.1.0 2020-09-23
------------------------
- Fixed quota change checking on updating file shares.
- Removed the help text in the message of without changes applicable to the
share on cid CLI.
- Fixed an incorrect variable replacement failure in the share removal function.

Release 1.0.9 2020-09-22
------------------------
- Fixed filtering of share names and paths with dollar ($).

Release 1.0.8 2020-09-20
------------------------
- Fixed logon with numeric accounts.
- Fixed management of numerical accounts in the system local groups.
- Added the additional config file option in the behavior options.
- Added the `-p` or `--preserve` option to the `join` command of the cid CLI
- Added enable/disable modes in the behavior options of the `join` command.
- Added enable/disable modes in the share options of the `share` command.
- Fixed the shares in Common mode.
- Merge of Common and WinACL sharing modes.
- Added `disk quota`, `tolerance quota` and `quota to the subdirs first-level` in
the options of the share manager.
- Added the share `template` option in the share manager.
- Added the additional `config file` option in the share manager.
- Allow updating of shares.
- Added the `wbd_userprofile`, `wbd_usershell`, `fallntpsrv`, `qttltime`,
`krb_principal_names` and `keytabfile` parameters in cid.conf.
- Removed unnecessary settings in the global section of the Samba config file.
- Fixed the documentation path in the comment block made by CID in the changed
system configuration files.
- Removed the Changelog section of the CID documentation.

Release 1.0.7 2020-03-25
------------------------
- Return of compatibility with Fedora and CentOS.
- New parameter `localgroups` added to the cid.conf file.
- New configuration for the pam_winbind module in the PAM authentication stack,
defining it as the first module to be consulted.
- Removal of the `Help >> User Manual` option in cid-gtk.
- Added `Help >> About CID` option in cid-gtk.

Release 1.0.6 2019-11-12
------------------------
- Fixed output of `--version` or `-v` command-line option.

Release 1.0.5 2019-11-08
------------------------
- Removal of "ntp" and "ntpdate" from dependency checking of "INSTALL.sh" script.

Release 1.0.4 2019-06-02
------------------------
- Fixed configuration of shared directory paths with whitespaces.
- Added Principal Name "MariaDB" to Keytab.
- Updated the Print Server Configuration.
- Added keyutils to requirements.
- Fixed hostname variable.
- ntpd replaced by systemd-timesync.
- Temporarily removed Fedora and CentOS Support.

Release 1.0.3 2018-07-25
------------------------
- Add entire domain groups to the local administrative groups of the stations.
- Bash Completion Script for cid utility.
- Substitution of the winbind backend tdb by the autorid in the default
configuration of the join to domain.
- Removal of the winbind database files after disassociating the station from
the domain.
- New parameters `id_range_size` and `max_num_domains` added to the cid.conf file.
- `cupsbkddir`, `cupsbkddir`, `cupsfile`, and `sudofile` parameters removed from the
cid.conf file.
- `Manage domain users in local groups` option has been replaced by
`Manage domain accounts in local groups` in the cid-gtk tool.
- The `usersgrp` subcommand has been replaced by `account` in the cid utility.
- The `block` subcommand of the cid utility no longer receives the user or group
arguments, instead the user account name or group account name preceded by an
"@" must be entered directly.
- The winacl share mode no longer supports the hidden option. A "$" should be
used at the end of the share name to hide it.
- Common domain users are no longer automatically added to the local CUPS
administration group. The logon_root.sh script should now be used to manage
printers on the system during the logon of these users.
- Removed the setting for transparent authentication of print jobs on smb
connections due to printing command failures in some applications such as
web browsers. We now recommend using the IPP protocol instead of SMB.
- New variables added to the user's logon environment. See the Variables
section of this file for more details.
- The use of the ifconfig utility in the CID functions has been replaced by
the ip utility.
- Fixed the blank lines while configuring the hosts file.
- Added the function of auto-tuning of the permissions in the user's home
directory during logon.

Release 1.0.2 2017-08-17
------------------------
- Fixed created script to aid transparent authentication in CUPS print jobs
for smb connections.

Release 1.0.1 2017-08-05
------------------------
- New requirements: su and smbspool commands.
- Transparent authentication in print jobs over SMB connections:
- CID corrected the problem of requesting authentication in CUPS print jobs
for an SMB connection.
> The solution found is based on the creation of a shell script that replaces
the symbolic link usually created for smbspool binary in the CUPS backends
directory. This script passes the print job arguments to smbspool, running
it as the user owning the print job instead of root, which allows the
process to use kerberos methods to authenticate the job.
Ref: https://sourceforge.net/p/c-i-d/news/2017/08/transparent-authentication-in-print-jobs-over-smb-connections

Release 1.0.0 2017-07-29
------------------------
- Fixed LightDM Greeter's automatic configuration.
- Fixed the loss of ownership of domain admins group on printer drivers
directory for Windows clients when a network failure occurs at system boot.
- New standard to versioning.
> From now on, the program will have three fields in the version number
(cid-X.X.X.tar.gz), and the variations will occur regardless of the
content of the update, sequentially, from the last field to the first
(from right to left) whenever a field reaches the digit nine.
If you already have a CID version installed on your system that is less
than version 9.1, you will first have to upgrade to version 9.1 to install
the version of this package. You can also choose to remove your current
version and then install this package, however it is NECESSARY that in this
case you remove the system from the domain before uninstalling the program,
if you already have it used to join your system to an AD domain.
Version 9.1 is only a transition version to version of this package and will
be available for a short time on the project website. If you need the package
of some legacy version of the program, you should request contacting the
developer at the email address: emoraes25@gmail.com.
Ref: https://sourceforge.net/p/c-i-d/news/2017/07/new-standard-to-versioning

Release 9.1 2017-06-28
------------------------
- New behavior options added:
  - Share all printers on CUPS in cid-gtk;
  - --share-printers in cid;
  - Use keytab file method in cid-gtk;
  - --use-keytab in cid.
- New sharing modes added (WinACL and Printer mode).
- Automatic creation of keytab file during join to the domain.
- Added the managing of CUPS printer shares in Samba.
- He started to use the "homes" section in Samba instead of the
“%U” variable and the name "userfolder" in the "Userfolder" share mode.
- Replaces the owner group, from root to the default group of the domain
(domain users), in the shared directories of the users created by the
"Userfolder" share mode.
- New requirement: attr.

Release 9.0 2017-06-07
------------------------
- Changed the script name .logon_(root).sh to logon_root.sh.
- New requirements: acl, awk, cups, ifconfig, ntp, pkexec, systemd and xhost.
- Replaced the /usr/share/cid/cid.conf file by the /etc/cid/cid.conf file, and
added parameters to specify the paths of configuration files used by CID.
- Compatibility with other Linux distributions other than those based on Debian.
- Automatic correction of the domain NetBIOS name when different from the domain name.
- Allow you to remove the station from domain without authenticating a domain administrator.
- Replacement of gksudo and kdesudo with pkexec and the polkit API in cid-gtk.
- Changed window sizes of cid-gtk and cid-change-pass-gtk.
- New `Manage file shares` function in cid-gtk.
- New `Scripts log` option in the Help menu of cid-gtk.
- Added new information displayed in the `Station info` option of the Help menu
of cid-gtk. Now it also shows the paths of the system files used by CID and the
status of the other parameters found in the cid.conf file.
- Reduced the commands names of cid utility (the old commands continue to be accepted by the tool):
  - joinstation → join
  - leavestation → leave
  - blocklogon → block
  - unlocklogon → unlock
  - usersysgroups → usersgrp
- New `share` command for cid utility.
- New information displayed in the `status` command of the cid utility. Now it
also shows the paths of the system files used by CID and the status of the other
parameters found in the cid.conf file.
- Replaced /etc/init/cid (SysV format) for CID Init Scrit in systemd.
- Replaced the modification in the sudoers file by a custom file in the sudoers.d
directory (the behavior of the sudo command for local users is no longer changed).
- Replaced the modification in the profile file by a custom file in the profile.d directory.

Release 8.3 2017-01-24
------------------------
- Fixed the problem of not creating users home directory and not execution of logon
scripts in the ssh access and with the login program. PAM files desconfigured after boot.

Release 8.2 2017-01-22
------------------------
- Fixed the problem of not creating users home directory and not execution of logon
scripts in the ssh access.

Release 8.1 2017-01-15
------------------------
- Fixed the problem of not creating users home folder and not running logon scripts
when using the "login" program.
- Removed blank lines in the output of the `cid usersysgroups list` command.
- Translation of all program interface and documentation for the English language
(standard), and change of the date format of the log for the american standard
(YYYY-MM-DD), to adequate the program for the packaging policies Debian.

Release 8.0 2016-12-23
------------------------
- Creation of the command-line utilities, and division into 04 executables
(cid, cid-gtk, cid-change-pass and cid-change-pass-gtk).
- Creation the Program Metadata Configuration File (/usr/share/cid/cid.conf).
- Creation of the initialization script "/etc/init.d/cid", which monitors the
modifications made to the configuration files of the program packages used
by the CID, and makes other adjustments to the system, such as synchronization
of the system time with the DC, And updating the DNS record (if supported by the server).
- Creation of the log files, functions.log (log of the functions of the app)
and scripts.log (log of the logon scripts).
- Reformulation of the "Join the domain" option with the new Mode field that
activates behavior options of the station in Advanced Mode.
- New behavior options, Disable NetBIOS over TCP/IP, and Disable Kerberos
Authentication, to the detriment of the previous version that adopted
these settings by default.
- New options, “Change station behavior”, “Block logon” and “Unblock logon”.
- Replacing the "About the CID" menu from the "Help" menu with 03 new options
(Station info, Usermanual, and App Log).
- Possibility of running the cid-change-pass and cid-change-pass-gtk utilities
as superuser to change the password of other users.
- Support the general options for graphical mode executables in command line.
- New format for distribution of the application via PPA repository for Ubuntu
and derived distributions.
- New script for automatic unmount of NETLOGON after user logon process, so that
the privilege of executing the umount command is restricted only to this disassembly,
and prevent ordinary users from using it to unmount any other filesystem that is mounted.

Release 7.1 2016-07-24
------------------------
- Inversion in the check logic and update of the manual list of users
added to the system groups by the cid package post-installation script;
- Omission of error messages in /etc/profile during offline logins using
credential caching;
- Omission of error messages in connectivity tests with the domain controller
while running certain application features;
- Removal of the "cached_login" flag from the pam_winbind.so module in
reauthentication for the pam_mount module in /etc/pam.d/common-session;
- Updating the cid-change-pass script from the terminal interface to the
graphical interface.

Release 7.0 2016-06-13
------------------------
- Adding the "winbind expand groups" parameter in /etc/samba/smb.conf to
the default value of the Samba versions prior to 4.2.x to fix the fault
in the mappings with the "sgrp" control attribute in pam_mount;
- Set in the services search order configuration for the hosts database
in /etc/nsswitch.conf to correct the error in name resolution in unicast
domains of ".local" zone;
- Clearing the credential cache in the removal of the station in the domain;
- Addition of the NETLOGON environment variable for use in logon scripts;
- Addition of the cid-change-pass script;
- Addition of the Add/Remove users from local system groups function;
- Insertion of management to reinstallation and/or downgrade of the 
package in the installation script (INSTALL.sh);
- Insertion of the modes of entry into the domain (Standard Mode and Custom Mode),
allowing the flexibility of some configurations and the use of RFC2307;
- Removing unnecessary packages from the list of dependencies in the cid
package (Removed packages: libpam-krb5, krb5-config, smbclient);
- Replacement of pam_krb5.so with pam_winbind.so;
- Replacement in static mapping method of NETLOGON via fstab using fixed
credentials by dynamic mapping with pam_mount;
- Use of forms to fill in the entry and removal parameters of the station
in the domain (Impact: Incompatibility with zenity package versions prior to 3.4.0);
