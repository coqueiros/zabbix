# Note: choose your distro: https://www.zabbix.com/download

# I have: zabbix version: 7.2
# OS: Rocky Linux
# OS Version: 8
# Zabbix Component: Server, frontend, Agent
# Database: MySQL
# Web Server: Apache


#Install and configure Zabbix for your platform

#Install Zabbix repository
rpm -Uvh https://repo.zabbix.com/zabbix/7.2/release/rocky/8/noarch/zabbix-release-latest-7.2.el8.noarch.rpm
dnf clean all

#Switch DNF module version for PHP
dnf module switch-to php:8.2

#Install Zabbix server, frontend, agent
dnf install zabbix-server-mysql zabbix-web-mysql zabbix-apache-conf zabbix-sql-scripts zabbix-selinux-policy zabbix-agent

Note:Install database 
yum install mysql-server -y

#Create initial database
#Make sure you have database server up and running.
#Run the following on your database host.

# mysql -uroot -p
password
mysql> create database zabbix character set utf8mb4 collate utf8mb4_bin;
mysql> create user zabbix@localhost identified by 'password';
mysql> grant all privileges on zabbix.* to zabbix@localhost;
mysql> set global log_bin_trust_function_creators = 1;
mysql> quit;

#On Zabbix server host import initial schema and data. You will be prompted to enter your newly created password.
zcat /usr/share/zabbix/sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix

#Disable log_bin_trust_function_creators option after importing database schema.
mysql -uroot -p
password
mysql> set global log_bin_trust_function_creators = 0;
mysql> quit;

#Configure the database for Zabbix server
#Edit file /etc/zabbix/zabbix_server.conf
DBPassword=password

#Start Zabbix server and agent processes
#Start Zabbix server and agent processes and make it start at system boot.
systemctl restart zabbix-server zabbix-agent httpd php-fpm
systemctl enable zabbix-server zabbix-agent httpd php-fpm

#Open Zabbix UI web page
#The default URL for Zabbix UI when using Apache web server is http://host/zabbix


#User: Admin
#Pw:zabbix



























































