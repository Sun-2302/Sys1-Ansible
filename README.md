# Ansible 
Ansible is a popular open-source platform used to automate the management and configuration of IT systems. It is an orchestration tool that enables system administrators to deploy, configure and manage complex infrastructures in a simple and repeatable way.
# Automate the deployment of virtual hosts on Apache with Ansible

## Requirements
First, we need to install Ansible. <br><br>
**Ansible Installing**<br>
```
sudo apt install ansible
```

## How to use?
1. Clone this repository<br>
2. Execute the "deploy_apache.yml" file<br>
```
ansible-playbook deploy_apache.yml -i host
```
## Files
1. **script.yml** : Ansible playbook which is a list of tasks that automatically execute against hosts, to configure the Apache virtual hosts for the domain names. <br>
```
 ---
  - name: Deploy Apache virtualHost configuration
    hosts: all
    become: true
    tasks:
      - name: Install Apache
        apt:
          name: apache2
          state: present

      - name: Create root directory for virtual hosts
        file:
          path: /var/www/html
          state: directory

      - name: Copy virtual host configuration files
        template:
          src: ../templates/virtualhost.conf.j2
          dest: /etc/apache2/sites-available/{{ item }}.conf
        with_items:
          - api.hei.school
          - back.hei.school
          - front.hei.school
        vars:
          server_name: "{{ item }}"
          server_alias: "www.{{ item }}"
          document_root: "/var/www/html/{{ item }}"

      - name: Enable virtual hosts
        command: a2ensite {{ item }}.conf
        with_items:
          - api.hei.school
          - back.hei.school
          - front.hei.school

      - name: Restart Apache
        service:
          name: apache2
          state: restarted
```

2. **template.conf.j2** :  A Jinja2 template used to generate the Apache virtual host configuration files.<br>
```
<VirtualHost *:80>
    ServerName {{ server_name }}
    ServerAlias www.{{ server_alias }}
    DocumentRoot {{ document_root }}
</VirtualHost>
```
3. **host.txt**: An inventory file used by Ansible to specify the target hosts or groups of hosts on which the playbook will be executed
```
[webserver]
localhost ansible_connection=local
```
