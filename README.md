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
**-Domain Admins**
**-Enterprise Admins**
**-Schema Admins**

<img width="1617" height="855" alt="image" src="https://github.com/user-attachments/assets/9f8a6c6f-3374-4d90-9d99-ca2638ea39a7" />

**Schema Update**:Command Prompt as Administrator on a domain controller
- Extends the Active Directory schema by adding new classes and attributes required for Microsoft Exchange Server 2019.

```
D:\Setup.exe /IAcceptExchangeServerLicenseTerms_DiagnosticDataOFF /PrepareSchema
```
image3


**Prepare AD**:Command Prompt as Administrator on a domain controller
- Creates Exchange organization objects, security groups, and required permissions inside Active Directory.

```
D:\Setup.exe /IAcceptExchangeServerLicenseTerms_DiagnosticDataOFF /PrepareAD
```

**Prepare Domains**:Command Prompt as Administrator on a domain controller
- Configures Exchange permissions across all domains in the forest to ensure proper access for Exchange servers.

```
D:\Setup.exe /IAcceptExchangeServerLicenseTerms_DiagnosticDataOFF /PrepareAllDomains
```
image4
📌
> [!NOTE]
> The Exchange forest preparation commands must be executed from a domain-joined server where the Exchange installation ISO is mounted, since Setup.exe is required from the installation media path.
It is recommended to run these commands from the server where Exchange will be installed, using the mounted ISO (e.g., E:).
After completion, Active Directory replication will automatically synchronize the changes across all Domain Controllers.

### 2. Install Exchange Prerequisites
#### 1) Install Required Roles and Features: Powershell as Administrator 
```
Install-WindowsFeature Server-Media-Foundation, NET-Framework-45-Features, RPC-over-HTTP-proxy, RSAT-Clustering, RSAT-Clustering-CmdInterface, RSAT-Clustering-Mgmt, RSAT-Clustering-PowerShell, WAS-Process-Model, Web-Asp-Net45, Web-Basic-Auth, Web-Client-Auth, Web-Digest-Auth, Web-Dir-Browsing, Web-Dyn-Compression, Web-Http-Errors, Web-Http-Logging, Web-Http-Redirect, Web-Http-Tracing, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Lgcy-Mgmt-Console, Web-Metabase, Web-Mgmt-Console, Web-Mgmt-Service, Web-Net-Ext45, Web-Request-Monitor, Web-Server, Web-Stat-Compression, Web-Static-Content, Web-Windows-Auth, Web-WMI, Windows-Identity-Foundation, RSAT-ADDS
```
<img width="1622" height="611" alt="image" src="https://github.com/user-attachments/assets/2a2e0fba-3ea7-4f49-95dd-f07f37cad281" />




#### 2) Install Required Software:
Before installing Microsoft Exchange Server 2019, the following prerequisite software packages must be installed on the Exchange Server.

- **.NET Framework 4.8 (Offline Installer)**  
  https://support.microsoft.com/en-us/topic/microsoft-net-framework-4-8-offline-installer-for-windows-9d23f658-3b97-68ab-d013-aa3c3e7495e0

- **Visual C++ Redistributable Packages for Visual Studio 2013**  
  https://www.microsoft.com/en-us/download/details.aspx?id=30679

- **Unified Communications Managed API 4.0**  
  https://www.microsoft.com/en-us/download/details.aspx?id=34992

- **IIS URL Rewrite Module**  
  https://www.iis.net/downloads/microsoft/url-rewrite

  image2, image5
> [!IMPORTANT]
> You should restart server after finishing installation of required software
-----------------------------------------

### 3. Install Exchange Server 2019
Mount the Exchange 2019 ISO
Run Setup:Command Prompt as Administrator and navigate to the mounted ISO directory
D:\Setup.exe /IAcceptExchangeServerLicenseTerms_DiagnosticDataOFF /mode:Install /r:MB

image6

-----------------------------------------------------------------
