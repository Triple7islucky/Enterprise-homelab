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
