# Configure_Apache_MySQL_server

### Install and Configure Apache Server and connect MySQL Server

#### Install Apache (httpd) on RHEL & CentOS Stream 9

"""
yum install httpd -y
"""

### Install MySQL Client , PHP-MySQLnd and Firewall

"""
yum install mysql -y
yum install php -y
yum install php-mysqlnd -y
yum install firewalld -y
"""

### Now start services

"""
systemctl enable --now firewalld
systemctl enable --now httpd
"""

### TO change apache port

"""
vi /etc/httpd/conf/httpd.conf
"""

### and replace port

"""
Listen 80 -> Listen 8080
"""
