# Project-1

## Lamp Stack Implementation

### Step 1 - Installing Apache and Updating Firewall

Update a list of packages in package manager
`sudo apt update`

Run apache2 package installation
`sudo apt install apache2 -y`

Verify apache2 was installed successfully
`sudo systemctl status apache2`

Verify using curl
curl http://44.203.198.117:80

Curl via Public IP
<img width="1052" alt="Screenshot 2022-07-04 at 16 39 05" src="https://user-images.githubusercontent.com/105384023/177186405-251139e1-2101-4baf-adaf-d0504e7790fe.png">

### Step 2 - Install MySql
Install mysql
`sudo apt install mysql-server -y`
`sudo mysql_secure_installation`

Access the mysql db
`sudo mysql`

### Step 3 - Install PHP
Install PHP
`sudo apt -y install php libapache2-mod-php php-mysql`
`php -v`

<img width="719" alt="Screenshot 2022-07-04 at 16 57 58" src="https://user-images.githubusercontent.com/105384023/177188845-de5d1cac-ebef-4b82-aa6a-7518f1eb6f9d.png">

### Step 4 - Create a Virtual Host for Apache
Create a Virtual Host
<img width="809" alt="Screenshot 2022-07-04 at 16 41 34" src="https://user-images.githubusercontent.com/105384023/177189699-50fc269b-b76d-4467-8e39-01dbf4390942.png">

### Step 5 - Enable PHP on Website
Modify Apache precedence for php files

cat /etc/apache2/mods-enabled/dir.conf

<IfModule mod_dir.c>
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>

Create index.php

cat /var/www/projectlamp/index.php
<?php
phpinfo();

<img width="1068" alt="Screenshot 2022-07-04 at 17 10 34" src="https://user-images.githubusercontent.com/105384023/177190385-401fdfb1-ad15-405d-be7f-50587afe88ef.png">
