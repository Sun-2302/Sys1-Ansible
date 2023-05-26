# Ansible 
Ansible is a popular open-source platform used to automate the management and configuration of IT systems. It is an orchestration tool that enables system administrators to deploy, configure and manage complex infrastructures in a simple and repeatable way.
# Automate the deployment of virtual hosts on Apache with Ansible

## Requirements
First, we need to install Ansible. <br><br>
**Ansible Installing**<br>
``
sudo apt install ansible
``

## How to use?
1.Clone this repository<br>
2.Execute the "deploy_apache.yml" file<br>
``
ansible-playbook deploy_apache.yml -i hosts
``
