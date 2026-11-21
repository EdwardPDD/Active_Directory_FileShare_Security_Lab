# Active Directory & File Share Security Lab

## Objective

The purpose of this challenge was to secure user accounts and network file shares in a Windows Active Directory environment. The goal was to organize users into security groups, restrict workstation logons, and configure access to the HR and Accounting shares hosted on a Linux Fileshare server. This required working with Active Directory, Linux file permissions, and Samba to ensure users could only access the resources assigned to them.

## Challenge Documentation

This repository includes screenshots of the configurations, troubleshooting steps, and final results from the XP Cyber Range challenge. The full challenge report is also available for a detailed breakdown of the tools used and steps taken to complete the objectives.

[View Full Challenge Report (PDF)](Ative_directory_fileshare_PDf/EdwardPenaDeDios_NCPReport170489.pdf)

## Environment and Tools

- Windows Server – Active Directory Domain Controller
- Active Directory Users and Computers (ADUC)
- Linux Fileshare Server
- Samba (SMB)
- Linux Access Control Lists (ACLs)
- PowerShell and Linux Terminal
- `setfacl`, `getfacl`, `getent`, `id`, `testparm`, `systemctl`

## Implementation

### 1. Active Directory Security Groups
I first Created three security groups in Active Directory: HRSec, AccountingSec, and Miscellaneous. then assigned Sergio Chanel to HRSec, Brimlock Stones to AccountingSec, and the remaining non-administrative users to Miscellaneous. These groups were used to manage access to the department file shares instead of assigning permissions to individual users.

![HR Security Group](Screenshots/HRSec-securitygrp.png)

![HR Group Membership](Screenshots/HRSec-Group-Membership.png)

![Accounting Group Membership](Screenshots/AccountingSec-Group-Membership.png)

![Miscellaneous Group Membership](Screenshots/Miscellaneous-Group-Membership.png)

### 2. User Account and Logon Restrictions

I Restricted Sergio Chanel and Brimlock Stones to logging into Workstation-Desk only. Then Disabled Rob's domain account and also restored Gary Thatcher's membership in the Domain Admins group. These changes helped limit unnecessary access to domain resources while maintaining administrative access.

![Sergio Logon Restrictions](Screenshots/Sergio-Logon-Restrictions.png)

![Brimlock Logon Restrictions](Screenshots/Brimlock-Logon-Restrictions.png)

![Rob Account Disabled](Screenshots/Rob-Account-Disabled.png)

![Domain Admin Membership](Screenshots/Gary-Domain-Admin-Membership.png)

### 3. Linux Directory Permissions

I then Connected to the Linux Fileshare server to inspect the existing permissions on `/share/HR` and `/share/Accounting`. Used `getent` and `id` to verify that the Linux server recognized the Active Directory groups and their members. Configured ACLs using `setfacl` to grant the appropriate department groups read, write, and execute permissions while explicitly denying access to unrelated security groups. Used `getfacl` to verify the changes.

![Initial Directory Permissions](Screenshots/Initial-Directory-Permissions.png)

![Initial ACLs](Screenshots/Initial-Fileshare-ACLs.png.png)

![Restricted Directory ACLs](Screenshots/Restricted-Directory-ACLs.png)

![Configured HR and Accounting ACLs](Screenshots/HR&Accouting-Group-ACL-Configured.png)

### 4. Samba File Share Configuration

after I Modified the Samba configuration file (`smb.conf`) to restrict access to the HR and Accounting shares. Disabled guest access and configured `valid users` and `write list` to allow the corresponding department security groups and Domain Admins. Verified the configuration using `testparm` and restarted the Samba service to apply the changes.

![Initial Samba Configuration](Screenshots/Samba-Initial-Share-Configuration.png)

![HR Samba Configuration](Screenshots/HR-Samba-Access-Configuration.png)

![Accounting Samba Configuration](Screenshots/Accounting-Samba-Access-Configuration.png)

![Samba Validation](Screenshots/Samba-Configuration-Validation.png)

### 5. Troubleshooting and Verification

During testing, the challenge initially failed several file-share access checks despite the security groups and Samba configuration being in place. I verified group resolution, checked the Linux ACLs, and reviewed the share permissions to identify what was missing. The issue was resolved after explicitly restricting unrelated security groups and correcting access for Domain Admins. The final validation confirmed that the HR and Accounting shares had the required access restrictions.

![AD Group Resolution](Screenshots/AD-Group-Resolution.png)

![Fileshare ACL Permissions](Screenshots/fileshare-directory-acl-permissions.png)

## Results

Successfully completed all 7 security checks in the XP Cyber Range challenge, *Interns & HR on the Domain Controller [NG]*. The final configuration restricted department file shares to authorized security groups, preserved Domain Admin access, limited workstation logons, and disabled the specified domain account.

![Completed Challenge - 7/7](Screenshots/challenge-completed-7-of-7.png)

## Key Takeaways

This challenge gave me hands-on experience managing access controls across Windows Active Directory and Linux. I learned how Linux ACLs control permissions on directories while Samba controls access to those directories over the network. It also helped me understand how domain security groups can be recognized and used by a Linux server, and why both file-system permissions and network share permissions need to be configured correctly to secure shared resources.
