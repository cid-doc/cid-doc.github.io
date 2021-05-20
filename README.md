CID
===
**License: GPL-3+**

*Copyright (C) 2012-2021 Eduardo Moraes <<emoraes25@gmail.com>>*

---
[Homepage](https://c-i-d.sourceforge.io)
[Documentation](https://sourceforge.net/p/c-i-d/documentation)
[Discussion](https://sourceforge.net/p/c-i-d/discussion/)
[Donations](https://sourceforge.net/p/c-i-d/donate)
---

##DESCRIPTION
**CID** (*Closed In Directory*) is a set of bash scripts for inserting and managing Linux computers in **Active Directory** domains. Modifications made to the system allow Linux to behave like a Windows computer within AD.

<br />
##FEATURES
- Run logon scripts
- Automatically mount network shares (files and printers)
- Offline logon (Credential cache)
- Easily grant privileges to domain users (eg: access to the sudo command)
- Manage Samba shares (files and printers) through a graphical interface
- Apply disk quota per shared directory (such as Windows Server)

<br />
##REQUIREMENTS
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

###INSTALLATION OF THE REQUIREMENTS IN THE MAIN LINUX DISTRIBUTIONS

####Debian (>= 9):
    apt install passwd sudo acl attr systemd x11-xserver-utils policykit-1 zenity iproute2 iputils-ping keyutils krb5-user libnss-winbind libpam-winbind samba-common-bin samba-dsdb-modules samba-vfs-modules smbclient samba cifs-utils libpam-mount cups-daemon cups-client

####Fedora (>= 30):
    dnf install shadow-utils sudo acl attr systemd xorg-x11-server-utils zenity iproute iputils keyutils krb5-workstation gvfs-smb samba-winbind samba-winbind-clients samba-client samba cifs-utils pam_mount cups cups-client

####OpenSUSE (>= 15):
	zypper install sudo acl attr systemd xhost zenity iproute2 iputils keyutils krb5-client samba-dsdb-modules gvfs-backend-samba samba-winbind samba-client samba cifs-utils pam_mount cups-client cups

####CentOS (= 7):
    yum install epel-release
    yum install shadow-utils sudo acl attr systemd xorg-x11-server-utils zenity iproute iputils keyutils krb5-workstation gvfs-smb samba-winbind samba-winbind-clients samba-client samba cifs-utils pam_mount cups

<br />
##INSTALLATION
**Installation requires root privileges!**
Run the **INSTALL.sh** file (in this directory) as follows:

    	./INSTALL.sh

**Note 1:** Run **./INSTALL.sh uninstall** to uninstall the program files from the same version of the package.

**Note 2:** In *Ubuntu* and its derivations it is possible to install the CID through packages available in the *PPA* repository. These packages contain the requirements marked as dependencies, which allows them to be automatically installed.The following are the commands for installing these packages:

    	add-apt-repository ppa:emoraes25/cid && apt update
    	apt install cid cid-gtk