# Basic Network Device Configuration (Router & Switch)

## Objective

Configured basic router and switch settings in Cisco Packet Tracer using CLI commands to understand device initialization, access security, and management setup.

---

## Tools Used

- Cisco Packet Tracer
- Cisco IOS CLI

---

## Devices Configured

- Router
- Layer 2 Switch

---

## Tasks Performed

### Router Setup

- Entered privileged EXEC mode
- Entered global configuration mode
- Renamed router hostname
- Configured enable secret password
- Configured console line password
- Configured VTY remote access lines
- Enabled encrypted password storage
- Added warning banner
- Saved running configuration

### Switch Setup

- Entered privileged EXEC mode
- Renamed switch hostname
- Configured enable secret password
- Set console password
- Set VTY password
- Enabled password encryption
- Added MOTD banner
- Assigned management IP to VLAN 1
- Enabled VLAN interface
- Saved configuration

---

## Key Commands Practiced

```bash
enable
configure terminal
hostname
enable secret
line console 0
line vty 0 15
password
login
service password-encryption
banner motd
interface vlan1
ip address
no shutdown
copy running-config startup-config
```
--- 

## Key Learnings

- Hostnames help identify network devices.
- Enable secret protects privileged access.
- Console and VTY lines control local/remote login.
- Password encryption improves security.
- VLAN management IP enables switch administration.
- Saving configuration prevents loss after reboot.

---

## SOC Relevance

Understanding device configuration helps SOC analysts during:

- Network incident investigations
- Access control reviews
- Misconfiguration detection
- Unauthorized remote access checks
- Security hardening validation

---

## Status

Completed as part of networking and SOC foundation learning.
