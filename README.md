# Enterprise AD Lab: Windows Server 2022 Core & pfSense Integration
## Project Objective
To design and deploy a secure, virtualized domain environment using Windows Server 2022 Core (Headless) and Windows 10 Pro, fully integrated behind a pfSense firewall. This project focuses on Infrastructure-as-Code principles using PowerShell for all configurations.

## Tech Stack
**OS:** Windows Server 2022 Standard (Core), Windows 10 Pro

**Security:** pfSense Firewall (Routing, DNS Resolver, Rules)

**Hypervisor:** VMware Workstation

**Admin Tools:** PowerShell (CLI-only management)

## Architecture & Network Design
**Domain:** mylab.local

**Network Segment:** 192.168.10.0/24

**DC01 (Domain Controller):** 192.168.10.10

**WIN10-01 (Workstation):** 192.168.10.20

**Gateway:** 192.168.10.1 (pfSense)

## Key Challenges & Troubleshooting (The "Real World" Work)
### 1. The "Black Box" Network Effect
**The Problem:** Initial domain-join attempts failed with Domain does not exist despite successful ICMP (Pings).

**The Discovery:** Used a staged isolation strategy. By moving hosts to a pure VMware LAN Segment, I identified a rare Subnet Mask reset (/32) and pfSense DNS Rebind Protection as the dual root causes.
**The Fix:** Corrected host subnetting to /24.

Configured pfSense to Disable DNS Rebind Checks for internal domain resolution.

### 2. Headless AD Promotion
The Process: Since I used Server Core, I managed the entire AD DS promotion via PowerShell, including Forest creation and DNS forwarder configuration to 8.8.8.8.

## Detailed Project Documentation
## Phase 1: Deployment & Initial Hurdles
**Incident:** Boot Loops & Administrator Lockout

The initial deployment resulted in repeated boot prompts and password rejection.

![image_alt](https://github.com/Shadow-cyber-pk/pfsense-Multi-subnet-Lab/blob/7e2d8e411282dcd140623646e8e818f4c18dc030/images/Screenshot%202026-03-10%20005913.png)

**Troubleshooting:** Verified BIOS/UEFI settings and attempted accessibility bypass via utilman.exe.

**Resolution:** Performed a clean rebuild with EFI Secure Boot enabled to ensure local SAM database integrity.

## Phase 2: Active Directory Promotion & DNS Setup
**PowerShell Implementation:**

### PowerShell
### Install AD DS Role
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools

### Promote to Domain Controller
Install-ADDSForest -DomainName "mylab.local" -InstallDns:$true -Force:$true
## Phase 3: Root Cause Identification (Isolation Strategy)
When domain-join failed, I moved the VMs to a VMware LAN Segment to remove the firewall as a variable.

**Discovery 1:** ipconfig revealed a /32 mask on the server, isolating the host.

**Discovery 2:** The Windows Firewall was in "Public" profile, blocking necessary ports.

## Phase 4: pfSense Re-Integration & Hardening
Once joined, I moved the lab back behind pfSense and applied enterprise-level rules:

**DNS:** Enabled Disable DNS Rebind Checks to allow internal resolution.
![image_alt](https://github.com/Shadow-cyber-pk/pfsense-Multi-subnet-Lab/blob/ea66db5105e9870dc484dd38b3f69742cc7440e0/images/Screenshot%202026-03-16%20100852.png)

**Firewall Rules:** Created an Alias Group and permitted traffic for Kerberos (88), LDAP (389), and RPC High Ports.
![image_alt](https://github.com/Shadow-cyber-pk/pfsense-Multi-subnet-Lab/blob/ea66db5105e9870dc484dd38b3f69742cc7440e0/images/Screenshot%202026-03-16%20101011.png)

## Lessons Learned
Subnetting Is Foundational: A single /32 vs /24 error can fully isolate an enterprise host.

**Firewalls Default to Deny:** AD requires more than just "Ping." You must understand port-level flows (DNS, Kerberos, RPC).

**Headless Admin:** Operating exclusively via PowerShell is essential for modern data center management.

## Final Status
**Secure Channel:** Verified (via Test-ComputerSecureChannel)

**Internet Reachability:** Operational (via DNS Forwarding)

**Security:** Hardened (Traffic filtered via pfSense)
