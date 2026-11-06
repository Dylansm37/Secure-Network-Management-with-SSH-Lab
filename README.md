# Secure Network Management with SSH Lab

## Overview

This lab demonstrates secure remote management of Cisco routers and switches using SSH. SSH was configured on one router and two switches, with standard ACLs restricting remote management access to a designated IT workstation.

The lab also verifies that Telnet is disabled and console access remains available.

## Objectives

- Configure IP addressing on a router, switches, and end devices.
- Configure switch management interfaces (SVIs).
- Configure default gateways on the switches.
- Configure hostnames and domain names.
- Create local user accounts for SSH authentication.
- Generate 1024-bit RSA keys.
- Enable SSH version 2.
- Configure VTY lines to use local authentication.
- Configure standard ACLs to restrict VTY access to the IT workstation.
- Configure a five-minute idle timeout for remote sessions.

## IP Addressing

| **Device** | **Interface** | **IP Address** | **Subnet Mask** |
| ---------- | ------------- | -------------- | --------------- |
| R1         | G0/0          | 192.168.10.1   | 255.255.255.0   |
| SW1        | VLAN 1        | 192.168.10.2   | 255.255.255.0   |
| SW2        | VLAN 1        | 192.168.10.3   | 255.255.255.0   |
| PC-IT      | NIC           | 192.168.10.10  | 255.255.255.0   |
| PC-USER    | NIC           | 192.168.10.20  | 255.255.255.0   |

**Default Gateway:** 192.168.10.1

## Technologies

- Cisco IOS
- Secure Shell (SSH)
- RSA key generation
- Standard Access Control Lists (ACLs)
- Local user authentication
- VTY line security
- Packet Tracer

## Security Configuration

SSH was configured on R1, SW1, and SW2 using local username authentication and RSA keys.

Standard ACL 10 was configured to permit only the IT workstation:

```cisco
access-list 10 permit host 192.168.10.10
```

The ACL was applied inbound to the VTY lines using:

```cisco
access-class 10 in
```

Telnet was disabled by allowing SSH as the only remote access transport:

```cisco
transport input ssh
```

A five-minute idle timeout was configured to automatically disconnect inactive remote management sessions.

## Troubleshooting

Initially, RSA key generation failed because the router was still using the default hostname, `Router`.

Cisco IOS requires a hostname other than the default and a domain name before RSA keys can be generated. The hostname and domain name were configured before successfully generating the RSA keys.

```cisco
hostname R1
ip domain-name company.local
crypto key generate rsa
```

The RSA modulus was set to 1024 bits for Packet Tracer compatibility.

## Verification

Commands used:

1. Verify interface addressing:
```cisco
show ip interface brief
```

2. Verify SSH configuration:
```cisco
show ip ssh
```

3. Verify configured ACLs:
```cisco
show ip access-lists
```

4. Verify active SSH sessions:
```cisco
show users
```

5. Verify device configuration:
```cisco
show running-config
```

6. Test SSH connectivity from PC-IT:
```text
ssh -l netadmin 192.168.10.1
ssh -l netadmin 192.168.10.2
ssh -l netadmin 192.168.10.3
```

## Security Testing

- PC0 successfully established SSH sessions with all three Cisco devices.

<img width="704" height="718" alt="PC0_ssh" src="https://github.com/user-attachments/assets/18cf7ee8-f86a-41dc-b9a5-010275d0bff1" />

- PC1 was denied SSH access by the standard ACL.

<img width="699" height="717" alt="PC1_ssh" src="https://github.com/user-attachments/assets/2d251d32-24fd-43fe-a341-e1f9a7037af6" />

- Regular PC-to-PC communication was unaffected.

<img width="688" height="713" alt="PC1_ping" src="https://github.com/user-attachments/assets/d45ab772-8984-4251-ab91-f6a361f805bd" />
<img width="702" height="717" alt="PC0_ping" src="https://github.com/user-attachments/assets/354c5187-c304-47e4-905a-0600ae158add" />



## Files

- `lab.pkt`: Packet Tracer topology
- `configs/`: Router and switch configuration files
- `topology.png`: Network topology
- `troubleshooting.md`: Troubleshooting notes
