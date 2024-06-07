# Ansible-GNS3-Lab-Setup-part-2-Ansible-Install-and-Simple-Playbook-Execution-
Ansible GNS3 Lab Setup part 2 (Ansible Install and Simple Playbook Execution)


In this video I configure and explain the following:

- Installation of Ansible on Ubuntu Server
- Configuration of ansible inventory
- Configuration of ansible variables
- Configuration of an ansible playbook
- Run ansible playbook against multiple Cisco devices
  
use this guide:

https://docs.ansible.com/ansible/latest/getting_started/index.html

and this guide:

https://www.packetswitch.co.uk/ansible-with-cisco/


1) Open GNS3 and open the Ansible GNS3 Lab Setup part 1 project:

2) Power on all the devices, Ubuntu Server and Cisco devices

3) Verify you can ping from the Ubuntu Server to the Cisco devices

4) Make note of the account you have on the Cisco devices (you will add this to your Ansible configuration when accessing the devices while running your playbook)

5) Install Ansible on the Ubuntu Server:

sudo apt install ansible

6) 
