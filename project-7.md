# DEVOPS TOOLING WEBSITE SOLUTION

## Setup NFS Server
* sudo yum -y update
* sudo yum install nfs-utils -y
* sudo systemctl start nfs-server.service
* sudo systemctl enable nfs-server.service
* sudo systemctl status nfs-server.service
* sudo chown -R nobody: /mnt/apps
* sudo chown -R nobody: /mnt/logs
* sudo chown -R nobody: /mnt/opt
* sudo chmod -R 777 /mnt/apps
* sudo chmod -R 777 /mnt/logs
* sudo chmod -R 777 /mnt/opt
* sudo systemctl restart nfs-server.service

## Configure access to NFS
* (opened exports)sudo vi /etc/exports
/mnt/apps <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)
/mnt/logs <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)
/mnt/opt <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)

* sudo exportfs -arv

### Checked Security Groups
* rpcinfo -p | grep nfs

## Prepare the Web Servers

* Launched a new EC2 instance with RHEL 8 Operating System
* sudo yum install nfs-utils nfs4-acl-tools -y

### Mounted /var/www/ and target the NFS server’s export for apps
* sudo mkdir /var/www
* sudo mount -t nfs -o rw,nosuid < NFS-Server-Private-IP-Address >:/mnt/apps /var/www

* < NFS-Server-Private-IP-Address >:/mnt/apps /var/www nfs defaults 0 0 
* sudo vi /etc/fstab


#### Install Remi’s repository, Apache and PHP

* sudo yum install httpd -y
* sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
* sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
* sudo dnf module reset php
* sudo dnf module enable php:remi-7.4
* sudo dnf install php php-opcache php-gd php-curl php-mysqlnd
* sudo systemctl start php-fpm
* sudo systemctl enable php-fpm
* setsebool -P httpd_execmem 1

## Tooling Website Complete

![Completed Image](/images/project-7ss.PNG)