# Active Directory Domain Services (ADDS) Lab

## Overview
Built a Windows Server 2022 Active Directory environment to simulate enterprise identity and access management, including user provisioning, group policies, and domain-joined clients.

## Key Features
- Domain Controller deployment (ADDS)
- User and group management
- Domain-joined Windows 10 client
- DNS and authentication configuration

## Tech Stack
- Windows Server 2022 (Core)
- Active Directory
- Windows 10
- pfSense (Firewall & Gateway)

## Architecture
- Domain: mylab.local  
- Subnet: 192.168.10.0/24  
- Domain Controller: 192.168.10.10  
- Client Machine: 192.168.10.20  
- Gateway: 192.168.10.1 (pfSense)  

## Key Achievements
- Successfully deployed and configured ADDS using PowerShell (Server Core)  
- Joined windows 10 client to domain and validated authentication workflows    
- Configured DNS forwarding and internal name resolution

## Access Control & Policy Implementation

* Created multiple domain users and organized them into security groups (HR, IT)
* Configured SMB file shares and structured departmental folders
* Implemented NTFS and share-level permissions to enforce role-based access control (RBAC)
* Verified restricted access by testing with different user accounts
* Created and applied Group Policy Object (GPO) to restrict Control Panel access for standard users
* Used `gpupdate /force` to troubleshoot and validate policy application

## Additional Troubleshooting

* Identified inherited NTFS permissions allowing unintended access and corrected using `icacls`
* Resolved credential caching issues affecting permission testing
* Validated access behavior across different user roles (authorized vs unauthorized)


## Troubleshooting & Root Cause Analysis
- Diagnosed domain join failure caused by incorrect subnet mask (/32 instead of /24)  
- Resolved DNS rebind protection issue in pfSense affecting domain resolution  
- Identified firewall profile misconfiguration blocking AD-related ports  
- Used network isolation strategy to systematically identify root causes

## Advanced Troubleshooting & Hybrid Networking
- Implemented secure connectivity between on-prem AD environment and Azure VM using Tailscale
- Configured subnet routing to enable cross-network communication (192.168.10.0/24)
- Diagnosed and resolved multi-homed Domain Controller DNS registration issues
- Corrected duplicate A records causing AD domain discovery failures
- Validated AD services across networks (LDAP, Kerberos, SMB, RPC) using PowerShell-based testing
- Analyzed and documented limitations of Active Directory domain join over overlay VPN architectures (Tailscale)
- Implemented RPC port range restriction to troubleshoot remote domain join issues
- Performed end-to-end debugging using PowerShell, Test-NetConnection, and nltest

## Key Learnings
- Active Directory relies heavily on DNS and proper subnet configuration  
- Firewall rules must allow specific ports (Kerberos, LDAP, RPC)  
- PowerShell is essential for managing headless server environments  


**Headless Admin:** Operating exclusively via PowerShell is essential for modern data center management.

## Final Status
**Secure Channel:** Verified (via Test-ComputerSecureChannel)

**Internet Reachability:** Operational (via DNS Forwarding)

**Security:** Hardened (Traffic filtered via pfSense)

## Outcome
Successfully built and validated an enterprise-style Active Directory environment with secure networking, access control, and real-world troubleshooting scenarios.
