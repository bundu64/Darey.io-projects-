# WEB STACK IMPLEMENTATION (LEMP STACK)

LEMP is an open-source web application stack used to develop web applications.
The term LEMP is an acronym that represents L for the Linux Operating system,
Nginx (pronounced as engine-x, hence the E in the acronym) web server, M for 
MySQL database, and P for PHP scripting language.
How to start 
In order to complete this project i created an AWS acount after which i proceeded to create 
and EC2  Instance of t2.nano server with Ubuntu Server 22.04 LTS

## INSTALLING THE NGINX WEB SERVER.
NGINX is hight performance web server. Installing NGINX  will give me the privilege to display my
web sever to my visitors. 

To achive this, the following  steps was taking.

sudo apt update
sudo apt install nginx
sudo systemctl status nginx
curl http://107.23.126.10:80

<img width="1265" alt="Screenshot 2022-05-28 at 13 05 05" src="https://user-images.githubusercontent.com/105384023/172065834-f7aa80ae-0479-43fb-a3c8-4129cf2fb59b.png">

### INSTALLING MYSQL
MySQL: is an open-source relational database management system, after a successful installation 
the password was validated 

sudo apt install mysql-server
sudo mysql

<img width="553" alt="Screenshot 2022-05-28 at 12 28 42" src="https://user-images.githubusercontent.com/105384023/172067298-684c9ef7-cac5-4811-b7ab-c18a45cf2c03.png">

<img width="806" alt="Screenshot 2022-05-28 at 12 28 56" src="https://user-images.githubusercontent.com/105384023/172067539-a11bbc3f-420a-405c-9c27-0f72b08fada3.png">
sudo mysql_secure_installation
sudo mysql -p
mysql> exit

###  INSTALLING PHP
To generate a dynamic content for the web server we just configured we need to in PhP.
sudo apt install php-fpm php-mysql

<img width="964" alt="Screenshot 2022-05-28 at 12 32 46" src="https://user-images.githubusercontent.com/105384023/172067849-969b44eb-236d-4d61-88ad-eaf160ac0925.png">

### CONFIGURING NGINX TO USE PHP PROCESSOR

sudo mkdir /var/www/projectLEMP
sudo chown -R $USER:$USER /var/www/projectLEMP
sudo nano /etc/nginx/sites-available/projectLEMP
sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
sudo nginx -t
sudo unlink /etc/nginx/sites-enabled/default
sudo systemctl reload nginx

<img width="973" alt="Screenshot 2022-05-28 at 13 05 22" src="https://user-images.githubusercontent.com/105384023/172068959-3c0dc4ea-8bc5-4faf-b96d-5ee48e905405.png">

### TESTING PHP WITH NGINX

sudo nano /var/www/projectLEMP/info.php
<?php
phpinfo();
http://107.2.3.126.10/info.php
<img width="1581" alt="Screenshot 2022-05-28 at 13 06 06" src="https://user-images.githubusercontent.com/105384023/172069099-05aa60d2-6068-40b2-9e59-86b035b0be3d.png">
