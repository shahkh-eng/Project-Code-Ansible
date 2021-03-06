# Project-Code-Ansible
Ansible-Network Automation Framework  GNS3 Lab - SSH To a Cisco Router and Run Show Commands with Ansible Playbook
You can check my article on Linkedin for details on how to set up GNS lab and use this script. https://www.linkedin.com/pulse/ansible-network-automation-framework-gns3-lab-ssh-cisco-javed/

Note: Please note that below scripts/libraries or configurations are for learning purposes only, do not use them in production environment.

LICENSE: GPLv3

Introduction:
    Ansible is an open source automation tool, it uses a decentralized, agent less architecture (does not need to an agent on the managed node). It manages different nodes via SSH transport protocol. Ansible is build using Python. There are different versions of Ansible available please check the official website for more information link. Ansible core is an open source platform however Ansible has a commercial offering Ansible Tower with enterprise features e.g. RESTful API to execute Ansible playbooks etc.

Ansible uses control machine that can be our laptop, desktop or server. The control machine uses Ansible to distribute the configuration changes through SSH to the managed nodes (in our case a Network Router/Switch). Ansible is idempotent (means that if a task has already been done and the playbook is re-launched, it will not change anything since the task has already been executed).

We can install Ansible on Linux distributions e.g. Fedora, Red Hat Enterprise Linux, and CentOS operating systems in package form. Can install on Ubuntu and debian based distributions using the official package. Check the installation documentation, But as we are working on “Network Automation Appliance”, it’s already installed. We will use 5 Cisco (IOS) Routers inside GNS3 environment to get started at a very basic level, you can built on it step by step.

Ansible Components:

    Modules: Ansible modules are written in Python language they are referenced as tasks inside Ansible Playbook or inside Ansible ad-hoc cli tool using cli arguments.

    Inventory File: Inventory file is an ini-like file, it contains IP address/hostname (should be resolved by DNS) of the devices to be automated by Ansible control machine. Hosts or groups of hosts can be referenced in a playbook or from the Ansible ad-hoc CLI. Default hosts file is in “/etc/ansible/hosts”

    Playbook: It contains the Ansible domain specific language (DSL) and is written in YAML (Yet another markup language or yaml aint a markup language). Playbook contains automation instructions, it consists of plays that have task or tasks to be perform on the managed node.

    Configuration file: It controls how Ansible will run, it contains the default dictionary name and path for modules, inventory file etc. Default configuration file is in “/etc/ansible/ansible.cfg”.

Installation:

        Step 1: Install GNS3 GUI:

        ·        Check the GNS3 installation Requirements first

        ·        Go to link https://www.gns3.com/

        ·        Click on "Free Download Button" [Login Page will pop up, Need to login/register if not done before] It’s free.

        ·        Clink on Download GNS3 for Windows

        ·        Install the "All-In-One" file and keep the options to default

          *If you download solarwinds, you will be asked to install “.NET Framework” as well.

        ·        Continue to Setup Wizard



        Step 2: Install VMware Workstation Pro/VMware Player

        ·        Go to https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html and download the free "VMware workstation player".

        ·        Install the player with default settings

                  *VMware Player requires the installation of additional free software "VMware VIX" to work.

        ·        Go to https://www.gns3.com and click on "Download GNS3 VM"

        ·        Clink on Download button under "VMWARE workstation and Fusion" option

        ·        Extract the file

        ·        Check the computer/Laptop requirements for GNS3 VM

                  *Make sure that CPU is capable of hardware virtualization and it’s enabled in system's BIOS

        ·        Open VMware Player click on >File > Open navigate and select the GNS3 VM.

        ·        Keep the settings to Default.



        Step 3: Integration of GNS3 GUI with GNS3 VM

        ·        Go to GNS3 GUI and click

        - GNS3 VM

          -  Enable GNS3 VM

          - Virtualization Engine

        -         Select VMware Player

          -  Click Refresh button to display VM name if not shown

2.“Network Automation” appliance by GNS3 (It contains Tools like Python 2&3, Netmiko, NAPALM, pyntc and Ansible pre installed), Download link. Please check caveats and specific requirement for your lab setup before trying to use them. Download link https://www.gns3.com/marketplace/appliances/network-automation
3.Cisco “IOS” image for Cisco emulated devices.

Configuration Steps:

1.Uncomment the lines "auto eth0 and iface etho inet dhcp"in the “interfaces” file so that “Network Automation appliance” can get IP Address via DHCP
2.Turn on the appliance and check the IP Address by typing “ifconfig”
3.Configure the IP Address on R1-R5 “IOS” Router interfaces connected to appliance from the same subnet as the appliance, below is one example from R2

      ·R2(config)#interface fastEthernet 2/0

      ·R2(config-if)#ip address 192.168.122.22 255.255.255.0

      ·R2(config-if)#no shutdown
4.You have to configure R2 for SSHv2

    For Example:

    (config)#hostname (R2)
    (config)#ip domain-name python.com
    (config)#crypto key generate rsa (use 1024 bit)
    (config)#enable password cisco
    (config)#username gnslab password cisco
    (config)#username gnslab privilege 15
    (config)#ip scp server enable
    (config)#line vty 0 4
    (config-line)#login local
    (config-line)#transport input ssh
    (config-line)#exit
    
5.Try to ping and SSH the R2-f2/0 ip addresses from Network Automation appliance command line (Should work if above steps are correct).

Ansible Workflow:

        1.Playbooks are created in YAML language to describe the workflow.

        2.Playbooks are deployed to an Ansible control machine.

        3.Control machine runs the playbook, the modules (Python Code) are copied to the remote hosts.

        4.Finally Ansible runs those modules (python code to automate the tasks) on the remote device in case of Linux servers (Cisco IOS-XR also support this model) but with networking devices e.g. routers/switches etc the python script is run locally on the control machine and only the CLI commands are sent from the control machine to the managed device over SSH.

To execute the playbook:

        $ ansible-playbook -i (inventory) (file name).yml

Related Links:

https://www.youtube.com/channel/UCP7WmQ_U4GB3K51Od9QvM0w
Book “Python network programming conquer all your networking challenges with the powerful python language” by Abhishek Ratan, Eric Chou, Pradeeban Kathiravelu.
https://docs.ansible.com
https://docs.ansible.com/ansible/2.7/user_guide/playbooks_best_practices.html
https://www.rogerperkin.co.uk/network-automation/what-does-a-network-automation-engineer-do/
Book “Network Programmability and Automation: Skills for the Next-Generation Network Engineer” by Jason Edelman, Scott S. Lowe and Matt Oswalt


