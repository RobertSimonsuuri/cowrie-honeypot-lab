# Cowrie SSH Honeypot Lab

A practical cybersecurity lab exploring honeypot deployment, SSH activity monitoring, and session investigation.

## Project overview

This project uses Cowrie to simulate a Linux SSH server and record activity generated from a Kali Linux VM. The goal is to practise collecting evidence, reconstructing sessions, and explaining findings.

All activity is generated manually in an isolated lab. This honeypot is not exposed to the internet.

## Lab environment

| Component | Purpose |
|---|---|
| VirtualBox | Hosts the virtual machines |
| Ubuntu Server | Runs the honeypot |
| Cowrie 3.1.4 | Emulates an SSH service and records activity |
| Kali Linux | Generates controlled test activity |
| Python virtual environment | Keeps Cowrie dependencies separate from system packages |

## Network design

Both VMs connect to a VirtualBox Internal Network named `honeypot-lab`. Other network adapters are disabled.

| Machine | Interface | IPv4 address |
|---|---|---|
| Ubuntu / Cowrie | `enp0s3` | `10.10.10.10/24` |
| Kali Linux | `eth0` | `10.10.10.20/24` |

Static addresses provide predictable connectivity and persist across reboots. No gateway or DNS server is configured.

Cowrie listens on TCP port `2222`.

## Deployment

- Created a clean Ubuntu snapshot before installing Cowrie.
- Created a dedicated `cowrie` account without administrator privileges.
- Installed Cowrie and its dependencies in a Python virtual environment.
- Initialised Cowrie's configuration and data directories.
- Configured a dedicated test credential for the simulated shell.
- Verified connectivity from Kali to the honeypot.

## First test: SSH login and system discovery

The first test verified that Cowrie records authentication attempts and commands entered during an SSH session.

### Test sequence

1. Connected from Kali to `10.10.10.10:2222`.
2. Submitted an incorrect password.
3. Logged in using the configured lab credential.
4. Entered the following commands:

   ```bash
   whoami
   hostname
   pwd
   ls
   uname -a
   exit
