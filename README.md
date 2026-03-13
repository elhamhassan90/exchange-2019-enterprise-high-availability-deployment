# exchange-2019-enterprise-high-availability-deployment
exchange-2019-high-availability-infrastructure

## Project Overview

This project demonstrates the deployment of an Enterprise High Availability Email Infrastructure using Microsoft Exchange Server 2019.

The design includes Client Access services and Database Availability Group (DAG) to ensure fault tolerance and database replication.

## Architecture Overview

## Virtual Machine Specifications

| VM Name  | Role            | OS                      | RAM | Disk |   IP Address   |
|----------|-----------------|-------------------------|-----|------|----------------|
| DC01     | Domain Controller | Windows Server 2019   | 2GB | 40GB | 192.168.100.10 |
| EXCAS01  | Exchange CAS    | Windows Server 2019     | 3GB | 60GB | 192.168.100.21 |
| EXMBX01  | Mailbox + DAG   | Windows Server 2019     | 5GB |120GB | 192.168.100.30 |
| EXMBX02  | Mailbox + DAG   | Windows Server 2019     | 5GB |120GB | 192.168.100.31 |

## Technologies Used

- Microsoft Exchange Server 2019
- Windows Server 2019
- Active Directory Domain Services
- Database Availability Group (DAG)
- VMware Workstation
- PowerShell

## High Availability Design

Two Mailbox servers are configured in a Database Availability Group (DAG).

Mailbox databases are replicated between EXMBX01 and EXMBX02 to provide automatic failover in case of server failure.

## Deployment Steps

1. Prepare Active Directory Schema
2. Install Exchange Prerequisites
3. Install Exchange Server 2019
4. Configure Client Access Server
5. Configure Database Availability Group
6. Create Mailbox Databases
7. Configure Database Replication

----------------------------------------------------------------------

### 1. Prepare Active Directory 
##### Before running the Exchange forest preparation commands, a user account with the following privileges is required:
**- Domain Admins**
**- Enterprise Admins**
**- Schema Admins**

<img width="1617" height="855" alt="image" src="https://github.com/user-attachments/assets/9f8a6c6f-3374-4d90-9d99-ca2638ea39a7" />

**Schema Update**:Command Prompt as Administrator on a domain controller
- Extends the Active Directory schema by adding new classes and attributes required for Microsoft Exchange Server 2019.

```
Setup.exe /IAcceptExchangeServerLicenseTerms_DiagnosticDataOFF /PrepareSchema
```

**Prepare AD**:Command Prompt as Administrator on a domain controller
- Creates Exchange organization objects, security groups, and required permissions inside Active Directory.

```
Setup.exe /IAcceptExchangeServerLicenseTerms_DiagnosticDataOFF /PrepareAD
```

**Prepare Domains**:Command Prompt as Administrator on a domain controller
- Configures Exchange permissions across all domains in the forest to ensure proper access for Exchange servers.

```
E:\Setup.exe /IAcceptExchangeServerLicenseTerms_DiagnosticDataOFF /PrepareAllDomains
```

📌 Note for Documentation:

These commands were executed on a single Domain Controller with appropriate administrative privileges. Changes were then replicated automatically to all other Domain Controllers through Active Directory replication

### 2. Install Exchange Prerequisites
