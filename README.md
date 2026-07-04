# Windows Server 2019 Homelab — DNS Server Configuration

## Overview
This lab documents the setup of the DNS Server role on Windows Server 2019, as a prerequisite step before promoting the server to a Domain Controller (AD DS).

**Environment:**
- Hypervisor: VirtualBox
- OS: Windows Server 2019
- Network: Host-Only Adapter (192.168.56.0/24)
- Server IP: 192.168.56.115

## Objectives
- Install the DNS Server role
- Create a Forward Lookup Zone
- Create a Reverse Lookup Zone
- Add Host (A) and Pointer (PTR) records
- Verify resolution using `nslookup`

## Steps

### 1. Install DNS Server Role
Installed via Server Manager → Add Roles and Features → DNS Server.

### 2. Configure Static IP
Set a static IP (192.168.56.115) on the Ethernet adapter to ensure consistent name resolution for future domain services.

### 3. Create Forward Lookup Zone
- Zone type: Primary
- Zone name: `creativityhub.com`
- Purpose: resolves hostname → IP address

### 4. Add Host (A) Record
- Name: `dc01`
- IP: `192.168.56.115`

### 5. Create Reverse Lookup Zone
- Network ID: `192.168.56.0/24`
- Auto-generated zone: `56.168.192.in-addr.arpa`
- Purpose: resolves IP → hostname

### 6. Add PTR Record
- Host IP: `192.168.56.115`
- Host name: `dc01.creativityhub.com`

### 7. Verification

```
nslookup dc01.creativityhub.com
→ Address: 192.168.56.115

nslookup 192.168.56.115
→ Name: dc01.creativityhub.com
```

## Lessons Learned
- DNS zone names must follow proper domain formatting (e.g. `domain.com`) — a typo here (adding an extra suffix) affects every downstream record and is far easier to fix before AD DS installation than after.
- Reverse lookup zones aren't required for DNS to function, but PTR records matter heavily once AD DS is in place (Kerberos, replication, troubleshooting).

## Next Steps
- Rename server to final hostname
- Finalize static IP configuration
- Install AD DS role and promote to Domain Controller
