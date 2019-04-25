# Welcome to the Unblock Health Appliance Setup Framework (ASF)

## Server software requirements

Ubuntu [18.04 LTS minimal](https://help.ubuntu.com/community/Installation/MinimalCD) server or similar Linux distribution on a virtual or physical machine is required. Unless you're an expert Linux admin with [Ansible](https://www.ansible.com/) skills, use the [64-bit Ubuntu 18.04 "Bionic Beaver" mini.iso](http://archive.ubuntu.com/ubuntu/dists/bionic/main/installer-amd64/current/images/netboot/mini.iso) netboot image or [64-bit Ubuntu 18.04 Server](https://www.ubuntu.com/download/server) image in case netboot will not work in your environment. The mini.io netboot creates the smallest footprint server so it's the most secure and requires minimal hardening for security.

Setup a Windows Hyper-V, VMware, VirtualBox, or other hypervisor VM:

* RAM: minimum 2048  megabytes, preferably **4096 megabytes**
* Storage: minimum 32 gigabytes, preferably **256 gigabytes**
* Network: **Accessible outbound to the Internet** (both IPv4 and IPv6), inbound access not required
* Firewall Route: The publically accessible IP should point to this server, a Linux firewall is automatically managed by the appliance

## Setup the operating system

First, install Ubuntu 18.04.1 LTS or above *minimal server* on your preferred hypervisor. The smaller the footprint, the safer and more secure the appliance. You can use most of the defaults, but provide the following defaults when you are asked to make choices:

* Hostname: **sandbox** (devl), **validate** (QA) or **appliance** (production).
* Default User Full Name: **Admin User**
* Default User Name: **admin**
* Default User Password: **adminDefault!**
* Disk Partitioning: **Guided - use entire disk**
* PAM Configuration: **Install security updates automatically**
* Software Packages: *OpenSSH is the only package that must be installed by default*

NOTE: the user you create is called the *admin user* below. 

## Prepare your appliance for software

After Ubuntu operating system installation is completed, log into the server as the *admin user* (see above).

Bootstrap the core utilities:
	
    sudo apt update && sudo apt install net-tools curl -y && \
    curl https://raw.githubusercontent.com/unblock-health/appliance-setup/master/bin/bootstrap.sh | bash

After bootstrap.sh is complete, exit the shell.

## Review your specific appliance variables

After you've exited, log back in as the *admin user* and review the appliance.secrets.ansible-vars.yml file to customize it for your installation. 

    cd /etc/appliance-setup-framework/conf
create a new file using the below command
    sudo vi container.secrets.conf.jsonnet

Refer **container.secrets.conf.tmpl.jsonnet** to fill the keys (Key indicates the following things )

    domainName: "The domain as how we need to access the URLs related with this application",
    rootPassword: "The mysql root password",
    mysqlUser: "The mysql username",
    mysqlDatabase: "The mysql database name",
    mysqlPassword: "Mysql database password",
    sslEnable: "false",
    grafanaAdminUser: "admin",
    grafanaAdminPassword: "Grafana admin password",
    gitUserId: "Github user id to access the private repositries of the application",
    gitHubAccessToken: "Github Access token to access the private repositries of the application",
    installSecret: "The unblock health ticket install secret string, use a strong string",
    installName: "The title on broweser tab",
    adminFirstname: "Admin",
    adminLastname: "User",
    adminEmail: "The admin email address",
    adminUsername: "ostadmin",
    adminPassword: "The unblock health ticket admin password ",
    smtpHost: "The smtp server name using to send mails",
    smtpPort: "The smtp port number",
    smtpUser: "The smtp username using to send mails",
    smtpPassword: "The smtp password using to send mails"
 

## Install software

    cd /etc/appliance-setup-framework
    bash bin/setup.sh

After setup is completed, reboot the server (Docker setup will be incomplete without a reboot):

    sudo reboot

