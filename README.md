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

6) After install verify ansible with the following command:

ansible --version

7) perform and ls to see all the files created in the default ansible directory:

ls /etc/ansible

8) you will see a "hosts" file, this is the default inventory location. Inventories organize managed nodes in centralized files that provide Ansible with system information and network locations. Using an inventory file, Ansible can manage a large number of hosts with a single command.

9) Lets add Router 1 to the default location

vim /etc/ansible/hosts

R1 ansible_host=192.168.158.200 










10)
11) Create your Project folder then go to your new folder:

mkdir ansible_quickstart

cd ansible_quickstart

8) 
9)
10)
11) Create a file named inventory.ini in the ansible_quickstart directory

touch inventory.ini
ls 

9) Edit the inventory file and add the Cisco routers:



10) 


