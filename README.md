# cacti-0.0.8h

Step 01: 
Install CentOS v7 -Infrastructure Server

Step 02:
After installtion update the os

# yum update all -y


Step 03:
Then Reboot the Bost

Step 04:

Install Essential Package at once

# yum -y install mariadb-server php php-mysql net-snmp-utils rrdtool php-snmp gcc mariadb-devel net-snmp-devel autoconf automake libtool dos2unix wget help2man vim

Step 06:
Enable http and mariadb services so that they stay active after reboot

# systemctl enable httpd.service
# systemctl enable mariadb.service


Step 06:
Restart the http and mariadb service

# systemctl restart httpd.service
# systemctl restart mariadb.service


Step 07:
Download and setup cacti

# cd /var/www/html
# wget http://www.cacti.net/downloads/cacti-0.8.8h.tar.gz
# tar -xzvf cacti-0.8.8h.tar.gz
# ln -s cacti-0.8.8h cacti

Step 08:
Set Cronjobs and file permissions

# adduser -d /var/www/html/cacti -s /sbin/nologin sreejon
# echo "*/1 * * * * sreejon php /var/www/html/cacti/poller.php &>/dev/null" >> /etc/cron.d/cacti
# cd/var/www/html/cacti
# chown -R sreejon.apache rra log
# chmod 775 rra log


Step 09:
Setup Cacti Database
# /usr/bin/mysql_secure_installation

Enter Current password for root ( enter for none) - enter
set root password? [Y/N]- Y
new password: cacti@123
re-type new password: cacti@123
Remove anonymous users [Y/N] - y
Disallow root login remotely [Y/n] - Y
remove test database and access to it [Y/n] - Y
reload privilege tables now? [Y/N] - Y


# mysqladmin -u root -p create cactidb
# mysql -p cactidb < /var/www/html/cacti/cacti.sql
# mysql -u root -p
# GRANT ALL ON cactidb.* TO root@localhost IDENTIFIED BY 'Passw0rd123';
# flush privileges;
# exit

Step 10:

Edit the config php file

# cd/var/www/html/cacti/include/

# vim config.php

$database_type = "mysql";
$database_default = "cactidb";
$database_hostname = "localhost";
$database_username = "root";
$database_password = "passw0rd123";
$database_port = "3306";
$database_ssl = false;

$url_path = "/cacti/";


Step 11:
Firewall Rules 

# firewall-cmd --permanent --zone=public --add-service=https
# firewall-cmd --permanent --zone=public --add-service=http
# firewall-cmd --reload

Step 12:'
Important PHP settings for time and log

# vim /etc/php.ini

date.timezone = Asia/Dhaka - (set time/date) + uncomment
error_log=syslog

Step 13:

Hit your server IP and add "/cacti" at the end to your ip address




use WINSCP Software

