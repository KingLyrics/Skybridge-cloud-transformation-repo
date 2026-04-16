# Phase 1 – On-Prem Infrastructure Setup

## Synopsis

I am acting as a Cloud Systems Engineer assigned to a client onboarding project.

**Objective:** Build a foundational on-premises infrastructure for SkyBridge Logistics that can later be migrated into a hybrid cloud environment.

---

## Environment Structure

**Company:** SkyBridge Logistics
**Network:** 192.168.10.0/24
**Domain:** skybridge.local

### Systems

* **DC01** → Domain Controller (Active Directory Domain Services, DNS)
* **FS01** → Linux File/Application Server
* **CL01** → Domain-joined Client Workstation

---

## Windows Server (DC01) Configuration

| Setting | Value                                 |
| ------- | ------------------------------------- |
| Name    | DC01                                  |
| OS      | Windows Server 2022/2025              |
| RAM     | 4 GB                                  |
| CPU     | 2 cores                               |
| Disk    | 50 GB                                 |
| Network | Host-Only (internal) + NAT (internet) |

### Network Configuration

* IP Address: 192.168.10.10
* Subnet Mask: 255.255.255.0
* DNS Server: 192.168.10.10
* Default Gateway: Not configured (host-only network)

A second network adapter was configured using NAT to provide outbound internet access.

---

## Active Directory Design

### Organizational Units (OUs)

* Finance
* HR
* IT

### Users

* john.finance
* mary.hr
* admin.it

For this lab:

* Password change at next logon was disabled
* Password expiration was disabled

### Security Groups

* Finance_Group
* HR_Group
* IT_Group

Users were assigned to their respective groups.

The **admin.it** account was added to the built-in **Administrators** group to allow administrative control on domain-joined systems.

---

## File Server Configuration (DC01 - Temporary)

A centralized file share structure was created:

```
C:\Shares\Finance
C:\Shares\HR
```

### Permissions Strategy

* Removed default "Everyone" access
* Applied **group-based access control**:

  * Finance_Group → Access to Finance folder
  * HR_Group → Access to HR folder

### Result

Users can only:

* View folders they have access to
* Access resources permitted to their role

This demonstrates **Role-Based Access Control (RBAC)** and secure file sharing practices.

---

## Windows Client (CL01) Configuration

| Setting | Value           |
| ------- | --------------- |
| Name    | CL01            |
| OS      | Windows 11      |
| RAM     | 4 GB            |
| CPU     | 2 cores         |
| Disk    | 50 GB           |
| Network | Host-Only + NAT |

### Network Configuration

* IP Address: 192.168.10.20
* DNS Server: 192.168.10.10

CL01 was successfully joined to the **skybridge.local** domain and authenticated using domain credentials.

---

## Connectivity Testing

* Verified communication between all systems using ICMP (ping)
* Confirmed:

  * DC01 ↔ CL01 connectivity
  * CL01 ↔ FS01 connectivity
  * Internet access via NAT adapter

---

## Access Control Validation

Tested user permissions from CL01:

* john.finance → Access to Finance folder only
* mary.hr → Access to HR folder only

Folders not authorized are not visible, demonstrating secure access enforcement.

---

## Linux Server (FS01) Setup

| Setting | Value           |
| ------- | --------------- |
| Name    | FS01            |
| OS      | Ubuntu Server   |
| RAM     | 4 GB            |
| CPU     | 2 cores         |
| Disk    | 20 GB           |
| Network | Host-Only + NAT |

### Configuration Tasks

* Set hostname to FS01
* Set static IP address:
    * IP address: 192.168.10.30
    * Gateway: 192.168.10.10
    * Ping 192.168.10.10 successful
* Installed OpenSSH Server
* Enabled ssh and accessed file server from Client machine
* Installed Samba (for future file services)

---

## Key Outcomes

* Successfully deployed a domain-based infrastructure
* Implemented Active Directory with structured users and groups
* Configured secure file sharing using RBAC principles
* Established internal networking with external internet access via NAT
* Validated authentication and access control across systems

---

## Issues Encountered & Resolutions

| Issue                    | Cause                               | Resolution                                      |
| ------------------------ | ----------------------------------- | ----------------------------------------------- |
| No internet connectivity | NAT adapter missing default gateway | Reset VMware NAT network and renewed DHCP lease |

---


