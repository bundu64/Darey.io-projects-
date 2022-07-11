# Project-7

## Devops Tooling Website Solution

In this project the following tiers were utilized for the servers 


DBServer Tier-  
NFS Server Tier- 
WebServer Tier- 

### Step 1 - Prepare a Web server

Create LVM on the NFS server disk

sudo yum update -y
lsblk
gdisk /dev/xvdb
gdisk /dev/xvdc
gdisk /dev/xvdd

vgcreate nfs-vg /dev/xvdb1 /dev/xvdc1 /dev/xvdd1
lvcreate -n lv-apps -L 2G nfs-vg
lvcreate -n lv-logs -L 2G nfs-vg
lvcreate -n lv-opt -L 2G nfs-vg
df -h
```

<img width="544" alt="Screenshot 2022-07-08 at 16 30 25" src="https://user-images.githubusercontent.com/105384023/178146668-25150d00-bcc6-48b6-b81d-8c5a16753785.png">


Format the filesystem for the lvm volumes

mkfs -t xfs /dev/nfs-vg/lv-apps
mkfs -t xfs /dev/nfs-vg/lv-logs
mkfs -t xfs /dev/nfs-vg/lv-opt
blkid


Set the file system to automount on startup
```
vi /etc/fstab
mkdir -p /mnt/apps
mkdir -p /mnt/logs
mkdir -p /mnt/opt
cat /etc/fstab
mount -a
df -h




Install NFS on the server
```
sudo yum -y update
sudo yum install nfs-utils -y
sudo systemctl start nfs-server.service
sudo systemctl enable nfs-server.service
sudo systemctl status nfs-server.service
sudo yum install nfs-utils -y

<img width="536" alt="Screenshot 2022-07-08 at 16 29 32" src="https://user-images.githubusercontent.com/105384023/178146855-25958179-4627-4397-ac14-b42794bbebaa.png">
Set up the file exports to allow access from the web server subnet (172.31.32.0/20)
```
vi /etc/exports
/mnt/apps (rw,sync,no_all_squash,no_root_squash)
/mnt/logs (rw,sync,no_all_squash,no_root_squash)
/mnt/opt (rw,sync,no_all_squash,no_root_squash)
sudo exportfs -arv



### Step 2 - Install Mysql on DB Server
Install MySql service
```
sudo hostnamectl set-hostname mysql-server
bash
sudo yum -y install https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm
sudo amazon-linux-extras install epel -y
sudo rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
sudo yum -y install mysql-community-server
sudo systemctl enable --now mysqld
sudo systemctl status mysqld
```
Configure and Secure MySql Service
```
sudo grep 'temporary password' /var/log/mysqld.log
sudo mysql_secure_installation -p'password'
```
Create the database
```
mysql -u root -p
CREATE DATABASE tooling;
CREATE USER `webaccess`@``IDENTIFIED WITH mysql_native_password BY 'password';
GRANT ALL ON tooling.* TO 'webaccess'@``;
FLUSH PRIVILEGES;
SHOW DATABASES;
exit
```
<img width="492" alt="Screenshot 2022-07-08 at 16 44 57" src="https://user-images.githubusercontent.com/105384023/178146968-f81c8a94-9613-4da3-919a-139b69058828.png">



### Step 3 - Launch and Install the Web Servers

sudo hostnamectl set-hostname webserver_3
bash
sudo yum install nfs-utils nfs4-acl-tools -y

Verify connectivity to the NFS Server

rpcinfo -p  | grep nfs

### Create the mount point for the nfs server

sudo mkdir /var/www
sudo mount -t nfs -o rw,nosuid 172.31.28.63:/mnt/apps /var/www
sudo vi /etc/fstab
    :/mnt/apps /var/www nfs defaults 0 0
    :/mnt/logs /var/log/httpd nfs defaults 0 0

### Install httpd on the webserver

sudo yum install httpd -y
sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm -y
sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm -y
sudo dnf module reset php -y
sudo dnf module enable php:remi-7.4 -y
sudo dnf install php php-opcache php-gd php-curl php-mysqlnd -y
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
sudo setsebool -P httpd_execmem 1

#### Clone the required repository and copy the html folder

yum install git -y
git clone https://github.com/stwalez/tooling.git
cd tooling /
cp -r html /var/www/html

#### Edit the mysql connection details in the php file - /var/www/html/functions.php

$db = mysqli_connect('', 'webaccess', '', 'tooling','');


#### Configure SELinux to permit the httpd service to use NFS

sudo setsebool -P httpd_use_nfs 1

Configure SELinux to permit the httpd service to utilize /usr/sbin/php-fpm service to connect to the db

#### setsebool -P httpd_can_network_connect_db 1
