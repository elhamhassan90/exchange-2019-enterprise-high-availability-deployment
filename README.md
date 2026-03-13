# exchange-2019-enterprise-high-availability-deployment
exchange-2019-high-availability-infrastructure

## Project Overview

This project demonstrates the deployment of an Enterprise High Availability Email Infrastructure using Microsoft Exchange Server 2019.

The design includes Client Access services and Database Availability Group (DAG) to ensure fault tolerance and database replication.

## Architecture Overview

## Virtual Machine Specifications

| VM Name  | Role            | OS                      | RAM | Disk | IP Address   |
|----------|-----------------|-------------------------|-----|------|--------------|
| DC01     | Domain Controller | Windows Server 2019   | 2GB | 40GB | 192.168.1.10 |
| EXCAS01  | Exchange CAS    | Windows Server 2019     | 3GB | 60GB | 192.168.1.20 |
| EXMBX01  | Mailbox + DAG   | Windows Server 2019     | 5GB |120GB | 192.168.1.30 |
| EXMBX02  | Mailbox + DAG   | Windows Server 2019     | 5GB |120GB | 192.168.1.31 |

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
