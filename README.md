# **Three-Tier Application Deployment Using Ansible**

### Overview

This repository demonstrates the automated deployment of a **three-tier application** using **Ansible**. 

The three-tier architecture includes:

1)**Web Server**: Running Apache Web Server

2)**Application Server**: Running Python with Flask

3)**Database Server**: Running MySQL

### Key Features:

Passwordless SSH authentication.

Installation and configuration of Apache Web Server, Python, Flask, and MySQL.

The use of roles to modularize the tasks for each server type.

### Key Files:

**ansible.cfg**: Configuration file for Ansible, specifying the inventory, private key file for SSH, and privilege escalation settings.

**inventory.ini**: Ansible inventory file specifying the IPs and hostnames for the web, app, and db servers.

**Roles Folder**: Contains roles for each server type:

       webserver/: Configures the Apache Web Server.
       appserver/: Installs and configures Python and Flask.
       dbserver/: Installs MySQL Server and necessary Python modules.
        
**site.yml**: The main Ansible playbook which calls the roles to configure each server.

## Setting Up Passwordless SSH
For automation to work without requiring passwords, passwordless SSH authentication is set up between the Ansible control machine and the target servers (web, app, db). This is achieved by copying the control machine’s SSH public key to the target servers.

Run the following on your control machine to set up SSH access:

        ssh-copy-id -i ~/.ssh/ansible_key user@web_server_ip
        ssh-copy-id -i ~/.ssh/ansible_key user@app_server_ip
        ssh-copy-id -i ~/.ssh/ansible_key user@db_server_ip

## Roles Breakdown
**1. Web Server (Apache)**

The webserver role sets up an Apache web server on the designated hosts, installs necessary packages, and deploys a basic HTML page.

Tasks:

        Install Apache Web Server (apache2 package)
        Ensure Apache service is started and enabled to run on boot.
        Deploy a simple HTML page at /var/www/html/index.html.
        
**2. Application Server (Flask)**

The appserver role sets up Python, installs Flask, and prepares the server to run a Flask-based application.

Tasks:

        Install Python 3 (python3 package).
        Install pip for Python 3.
        Install Flask using pip.

**3. Database Server (MySQL)**

The dbserver role installs MySQL Server, starts the service, and ensures the necessary dependencies (such as PyMySQL) are installed.

Tasks:

        Install MySQL Server (mysql-server package).
        Install PyMySQL Python module for interaction with MySQL.
        Ensure MySQL service is started and enabled to run on boot.

## Playbook Execution

Playbook File (site.yml)

This playbook includes all the roles and runs them on the respective servers.Once after set up of inventory and configuring the roles, execute the playbook with the following command:

        ansible-playbook -i inventory.ini playbook.yml

## Conclusion

This project automates the setup of a three-tier architecture using Ansible. By utilizing roles, playbooks, and proper configuration management, this solution ensures that the application environment is reproducible, scalable, and easy to manage.

This setup can be extended to include more advanced configurations or additional tiers (e.g., caching servers, load balancers). The modular nature of Ansible roles also allows for easy updates or changes to the environment with minimal disruption.
