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

4) Make note of the user account you have on the Cisco devices (you will add this to your Ansible configuration when accessing the devices while running your playbook)

5) Install Ansible on the Ubuntu Server:

apt-add-repository ppa:ansible/ansible

![image](https://github.com/bowlercbtlabs/Ansible-GNS3-Lab-Setup-part-2-Ansible-Install-and-Simple-Playbook-Execution-/assets/120626722/e8ddc48d-13a2-41f9-a7ac-62199773b8e0)

apt update

![image](https://github.com/bowlercbtlabs/Ansible-GNS3-Lab-Setup-part-2-Ansible-Install-and-Simple-Playbook-Execution-/assets/120626722/0df49b89-e44a-48ff-be5c-f32b52dc3aa3)

apt install ansible

![image](https://github.com/bowlercbtlabs/Ansible-GNS3-Lab-Setup-part-2-Ansible-Install-and-Simple-Playbook-Execution-/assets/120626722/c26a3b0e-2d1b-41ef-94c9-bdb5dd26433e)

6) After install verify ansible with the following command:

ansible --version

![image](https://github.com/bowlercbtlabs/Ansible-GNS3-Lab-Setup-part-2-Ansible-Install-and-Simple-Playbook-Execution-/assets/120626722/aa4b1f2b-42cd-49bd-a06c-926198444c21)

7) perform and ls to see all the files created in the default ansible directory:

ls /etc/ansible

![image](https://github.com/bowlercbtlabs/Ansible-GNS3-Lab-Setup-part-2-Ansible-Install-and-Simple-Playbook-Execution-/assets/120626722/312f8980-8323-4c37-9297-0ca71bc5e75a)

8) you will see a "hosts" file, this is the default inventory location. Inventories organize managed nodes in centralized files that provide Ansible with system information and network locations. Using an inventory file, Ansible can manage a large number of hosts with a single command.

9) Lets add Routers 1,2 and 3 to the default inventory location along with our variables:

vim /etc/ansible/hosts

[cisco_routers]
R1 ansible_host=192.168.158.200
R2 ansible_host=192.168.158.201
R3 ansible_host=192.168.158.202

[cisco_routers:vars]
ansible_network_os=ios
ansible_user=steve
ansible_password=password
ansible_connection=network_cli

![image](https://github.com/bowlercbtlabs/Ansible-GNS3-Lab-Setup-part-2-Ansible-Install-and-Simple-Playbook-Execution-/assets/120626722/7c3974fd-b315-4b12-a882-3c0c0e9d6fba)


- "cisco_routers" is the group name that we will call inside our playbook creation later
- R1 is the alias for the host with IP address 192.168.158.200
- "cisco_routers:vars" is the group variables that we can use to tell ansible to use these credentials for all the devices inside the "cisco_routers" group. This helps further simplify our files by getting rid of repetative data entry.
- ansible_network_os - set to match your network platform you are communicating with
- ansible_user - the user account used to login to the host
- ansible_password - the password used to loing to the target host
- ansible_connection - the protocol/port used to connect to the target host

10) Power on the devices inside GNS3 now

![image](https://github.com/bowlercbtlabs/Ansible-GNS3-Lab-Setup-part-2-Ansible-Install-and-Simple-Playbook-Execution-/assets/120626722/252ef660-e757-4ffe-879b-ce253ed8acf2)

12) verify the inventory file with the following command:

ansible-inventory --list

![image](https://github.com/bowlercbtlabs/Ansible-GNS3-Lab-Setup-part-2-Ansible-Install-and-Simple-Playbook-Execution-/assets/120626722/0e17e1d3-523f-48fd-b871-9f69243a2483)

11) verify reachability to the routers by using the following ping command:

ansible cisco_routers -m ping

![image](https://github.com/bowlercbtlabs/Ansible-GNS3-Lab-Setup-part-2-Ansible-Install-and-Simple-Playbook-Execution-/assets/120626722/5fa020b3-af31-48d7-b695-f9d116cb82c6)

12) Lets build the playbook next, Playbooks are automation blueprints, in YAML format, that Ansible uses to deploy and configure managed nodes and can be further described as:

Play
An ordered list of tasks that maps to managed nodes in an inventory.

Task
A reference to a single module that defines the operations that Ansible performs.

Module
A unit of code or binary that Ansible runs on managed nodes. Ansible modules are grouped in collections with a Fully Qualified Collection Name (FQCN) for each module.

vim etc/ansible/cisco_playbook.yaml

![image](https://github.com/bowlercbtlabs/Ansible-GNS3-Lab-Setup-part-2-Ansible-Install-and-Simple-Playbook-Execution-/assets/120626722/e853fd60-9f82-44a7-908e-7dc70f45517a)

---

- name: My first Cisco Playbook (Show how long the router has been up)
  hosts: cisco_routers
  gather_facts: false

  tasks:
    - name: Show uptime of Cisco IOS routers
      ios_command:
        commands: show version | i uptime
      register: output

    - name: print output
      debug:
        var: output.stdout_lines


- hosts calls the cisco_routers group we specified in our inventory file, this is what devices the playbook will be run against
- gather_facts: false (we can use this to get information about the devices, this is turned on be default, so we have to turn it off with the command)
- name: self explanatory, used to describe the playbook/tasks
- ios_command: module used to run the cisco command
- 

![image](https://github.com/bowlercbtlabs/Ansible-GNS3-Lab-Setup-part-2-Ansible-Install-and-Simple-Playbook-Execution-/assets/120626722/1ba02bd8-5acf-458a-8203-39e6cdc3c4e7)

![image](https://github.com/bowlercbtlabs/Ansible-GNS3-Lab-Setup-part-2-Ansible-Install-and-Simple-Playbook-Execution-/assets/120626722/1da2233d-d752-483e-8497-788f2b5e62b2)

![image](https://github.com/bowlercbtlabs/Ansible-GNS3-Lab-Setup-part-2-Ansible-Install-and-Simple-Playbook-Execution-/assets/120626722/70fac2c4-2d78-42cb-b958-15a7ce774a26)


13) We can add another play to our playbook to include the Version of the router as well

![image](https://github.com/bowlercbtlabs/Ansible-GNS3-Lab-Setup-part-2-Ansible-Install-and-Simple-Playbook-Execution-/assets/120626722/d4ae1ecb-77ba-4e68-ad8c-0d9f5d9c5f3c)

![image](https://github.com/bowlercbtlabs/Ansible-GNS3-Lab-Setup-part-2-Ansible-Install-and-Simple-Playbook-Execution-/assets/120626722/c6625284-c20b-4022-86a5-a06d553ec982)

![image](https://github.com/bowlercbtlabs/Ansible-GNS3-Lab-Setup-part-2-Ansible-Install-and-Simple-Playbook-Execution-/assets/120626722/d0f21e9f-6dd4-44f6-a0a2-b6ffb3d79870)

![image](https://github.com/bowlercbtlabs/Ansible-GNS3-Lab-Setup-part-2-Ansible-Install-and-Simple-Playbook-Execution-/assets/120626722/07b10b2a-980b-47d6-a914-2c2ec4307b67)

![image](https://github.com/bowlercbtlabs/Ansible-GNS3-Lab-Setup-part-2-Ansible-Install-and-Simple-Playbook-Execution-/assets/120626722/794f4e40-c7dc-4480-87d4-05303b2372f3)

14) Another way to write the two plays so that they are shown together is:

![image](https://github.com/bowlercbtlabs/Ansible-GNS3-Lab-Setup-part-2-Ansible-Install-and-Simple-Playbook-Execution-/assets/120626722/5d3f09b8-9704-4e91-8d47-6373d1f2bd8a)

![image](https://github.com/bowlercbtlabs/Ansible-GNS3-Lab-Setup-part-2-Ansible-Install-and-Simple-Playbook-Execution-/assets/120626722/0be2776d-d804-4d5e-b725-0a919266152e)

![image](https://github.com/bowlercbtlabs/Ansible-GNS3-Lab-Setup-part-2-Ansible-Install-and-Simple-Playbook-Execution-/assets/120626722/2c55c720-160f-47e6-959f-eb4ed2611662)

15) Last thing we can do is show the IP address assigned to the router in which we are connecting to for verification:

![image](https://github.com/bowlercbtlabs/Ansible-GNS3-Lab-Setup-part-2-Ansible-Install-and-Simple-Playbook-Execution-/assets/120626722/0511eb05-f368-4ec2-a605-0be126ad4c4d)

![image](https://github.com/bowlercbtlabs/Ansible-GNS3-Lab-Setup-part-2-Ansible-Install-and-Simple-Playbook-Execution-/assets/120626722/eed722ca-55fd-47ef-be55-88c223452913)

16) That's it! Your first Ansible Playbook has been completed!

