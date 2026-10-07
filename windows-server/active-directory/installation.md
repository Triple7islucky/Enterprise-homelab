# Active Directory Domain Services Setup

## Objective

Configure DC01 as the first domain controller for the
corp.example.test Active Directory forest.

## Server Configuration

Hostname: DC01
Role: Domain Controller / DNS Server

## AD DS Installation

Active Directory Domain Services was installed through
Server Manager using the Add Roles and Features wizard.

A new Active Directory forest was configured using:

Domain: corp.example.test

## Issue: Domain Controller Promotion Prerequisite Failure

During the prerequisite check, domain controller promotion
failed because the local Administrator account had a blank
password.

### Error

The local Administrator account password did not meet the
requirements for creation of a new domain.

### Root Cause

The Windows Server installation had a local Administrator
account without a password. During creation of a new forest,
this account becomes the domain Administrator account.

### Resolution

A strong password was configured for the local Administrator
account.

The prerequisite check was then rerun before continuing the
domain controller promotion.

### Result

The Administrator account satisfied the password requirements
required for creation of the new Active Directory domain.

## Lessons Learned

Domain controller promotion performs prerequisite validation
before creating a new Active Directory forest. Administrative
accounts must satisfy the required security and password
requirements before promotion can proceed.

## Post-Installation Verification

After the server restarted following promotion to a domain controller, I verified that the core Active Directory services were running successfully.

The following services were confirmed to be running:

- Active Directory Web Services (ADWS)
- DNS Server
- Netlogon

I also performed DNS resolution tests to verify that the newly created Active Directory domain was functioning correctly.

The following were successfully tested:

- Resolution of `corp.example.test`
- Resolution of `DC01.corp.example.test`
- Active Directory LDAP SRV record lookup
- External DNS resolution

DC01 was configured with the static IPv4 address `192.168.254.10`. After promotion, the domain controller was configured to use its local DNS service.

These tests confirmed that Active Directory Domain Services and DNS were functioning correctly before additional domain configuration was performed.

## Result

DC01 is now operating as the first domain controller and DNS server for the `corp.example.test` Active Directory forest.

With the core domain infrastructure verified, the next phase of the project focuses on designing the organizational structure, security groups, user accounts, and role-based access controls.
