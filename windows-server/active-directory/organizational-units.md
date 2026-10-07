# Active Directory Organizational Unit Design

## Objective

After successfully deploying the `corp.example.test` Active Directory forest, I created a custom Organizational Unit (OU) hierarchy to simulate the structure of an enterprise environment.

The objective was to separate users, computers, administrative accounts, service accounts, and security groups into logical containers that can later be used for administration, delegation, and Group Policy.

## Organizational Structure

A top-level Organizational Unit named `Enterprise` was created within the `corp.example.test` domain.

The following structure was created:

```text
corp.example.test
│
├── Domain Controllers
│   └── DC01
│
└── Enterprise
    │
    ├── Users
    │   ├── IT
    │   ├── HR
    │   ├── Finance
    │   └── Operations
    │
    ├── Computers
    │   ├── Workstations
    │   └── Servers
    │
    ├── Groups
    ├── Admin Accounts
    └── Service Accounts
```

The default Active Directory containers and the `Domain Controllers` OU were left unchanged.

## Departmental User OUs

Four departmental OUs were created underneath the `Users` OU:

- IT
- HR
- Finance
- Operations

These OUs provide logical separation between employees belonging to different organizational departments.

This structure will also allow policies or delegated administrative responsibilities to be targeted toward specific departments later in the project.

## Computer OUs

The `Computers` OU was divided into:

- `Workstations`
- `Servers`

This separates employee/client computers from server systems.

Domain-joined workstations such as `WS01` will eventually be placed in the `Workstations` OU, while future member servers can be placed in the `Servers` OU.

## Administrative and Service Accounts

Separate OUs were created for:

- `Admin Accounts`
- `Service Accounts`

Administrative accounts will be separated from normal employee accounts so privileged identities can be managed independently.

Service accounts will be stored separately from interactive user accounts and will eventually be used for services or applications that require domain identities.

## Security Group Structure

A dedicated `Groups` OU was created to contain Active Directory security groups.

The following Global Security Groups were created:

```text
GG_IT_Users
GG_HR_Users
GG_Finance_Users
GG_Operations_Users
```

These groups will represent departmental membership.

The following Domain Local Security Groups were also created:

```text
DL_IT_Modify
DL_HR_Modify
DL_Finance_Modify
DL_Operations_Modify
```

The Global groups were nested into their corresponding Domain Local groups:

```text
GG_IT_Users
    ↓
DL_IT_Modify

GG_HR_Users
    ↓
DL_HR_Modify

GG_Finance_Users
    ↓
DL_Finance_Modify

GG_Operations_Users
    ↓
DL_Operations_Modify
```

## AGDLP Permission Model

This structure begins implementing the Microsoft Active Directory AGDLP permission model:

```text
Accounts
    ↓
Global Groups
    ↓
Domain Local Groups
    ↓
Permissions
```

Employee accounts will be assigned to Global groups based on their organizational role.

Global groups are then nested inside Domain Local groups associated with access to specific resources.

Permissions will eventually be assigned to the Domain Local groups instead of directly to individual users.

For example:

```text
HR Employee
    ↓
GG_HR_Users
    ↓
DL_HR_Modify
    ↓
HR Shared Resource
    ↓
Modify Permission
```

This provides a more scalable approach than assigning resource permissions individually to every employee.

## Current Status

The Active Directory organizational structure and initial departmental security groups have been created.

The next phase of the project will include:

- Creating test employee accounts
- Assigning users to departmental Global groups
- Creating separate privileged administrative identities
- Creating Help Desk roles
- Joining `WS01` to the domain
- Testing authentication and group membership
- Implementing resource permissions
- Implementing Group Policy
- Testing least-privilege access controls
