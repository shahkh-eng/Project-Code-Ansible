# Project-Code-Ansible
## Ansible-Network Automation Framework & GNS3 Lab

![Ansible](https://img.shields.io/badge/Ansible-Automation-EE0000?style=flat&logo=ansible&logoColor=white)
![Cisco IOS](https://img.shields.io/badge/Cisco-IOS-1BA0D7?style=flat&logo=cisco&logoColor=white)
![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)

**Ansible-Network Automation Framework: GNS3 Lab - SSH to a Cisco Router and Run Show Commands with Ansible Playbook.**

> 📖 **Read the original step-by-step LinkedIn walkthrough:**  
> 👉 [Ansible-Network Automation Framework GNS3 Lab - SSH To a Cisco Router and Run Show Commands with Ansible Playbook](https://www.linkedin.com/pulse/ansible-network-automation-framework-gns3-lab-ssh-cisco-javed/)

*Disclaimer: The scripts, libraries, and configurations provided in this repository are for educational and learning purposes only. Do not use them in a production environment without rigorous testing.*

---

## Introduction

Ansible is an open-source automation tool that uses a decentralized, agentless architecture (meaning it does not require an agent installed on the managed node). It manages remote nodes via the SSH transport protocol and is built using Python. 

Ansible uses a control machine (your laptop, desktop, or server) to distribute configuration changes through SSH to managed nodes (such as network routers and switches). Ansible is **idempotent**, meaning that if a task has already been completed and the playbook is re-launched, it will not make redundant changes.

We will use 5 Cisco (IOS) routers inside a GNS3 environment to get started at a foundational level, upon which you can build step by step.

---

## Ansible Components

- **Modules:** Ansible modules are written in Python and referenced as tasks inside an Ansible Playbook or executed via the Ansible ad-hoc CLI tool using arguments.
- **Inventory File:** An INI-like file containing the IP addresses or hostnames of the devices to be automated. Hosts or groups of hosts can be referenced in a playbook or from the CLI.
- **Playbook:** Written in YAML (YAML Ain't Markup Language), playbooks contain Ansible's domain-specific language (DSL) consisting of plays and tasks to perform on managed nodes.
- **Configuration File (`ansible.cfg`):** Controls how Ansible runs, including default dictionary names, paths for modules, and inventory files.

---

## Lab Setup & Prerequisites

### Step 1: Install GNS3 GUI
- Check GNS3 installation requirements first via [GNS3.com](https://www.gns3.com/).
- Download the "All-In-One" installer for your platform and keep options default.

### Step 2: Install VMware Workstation Pro / Player
- Download and install free VMware Workstation Player or Workstation Pro.
- Download the **GNS3 VM** image for VMware Workstation from GNS3.com and open it inside VMware. Ensure hardware virtualization is enabled in your system's BIOS.

### Step 3: Integration of GNS3 GUI with GNS3 VM
- Open GNS3 GUI $\rightarrow$ **GNS3 VM** $\rightarrow$ **Enable GNS3 VM** $\rightarrow$ Select your virtualization engine and click **Refresh**.
- Use the GNS3 **"Network Automation" appliance** (pre-installed with Python 2/3, Netmiko, NAPALM, pyntc, and Ansible) along with standard Cisco IOS images for R1–R5.

---

## Configuration Steps

1. Uncomment the lines `auto eth0` and `iface eth0 inet dhcp` in the appliance `/etc/network/interfaces` file so the Network Automation appliance gets an IP address via DHCP.
2. Turn on the appliance and verify the IP address using `ifconfig`.
3. Configure IP addresses on the R1–R5 Cisco IOS router interfaces connected to the appliance within the same subnet. Example for **R2**:
   ```cisco
   R2(config)# interface fastEthernet 2/0
   R2(config-if)# ip address 192.168.122.22 255.255.255.0
   R2(config-if)# no shutdown
