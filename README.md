# Configure_Apache_MySQL_server

## Install and Configure Apache Server and connect MySQL Server

## Apache Server

#### Install Apache (httpd) on RHEL & CentOS Stream 9

```
yum install httpd -y
```

### Install MySQL Client , PHP-MySQLnd and Firewall

```
yum install mysql -y
yum install php -y
yum install php-mysqlnd -y
yum install firewalld -y
```

### Now start services

```
systemctl enable --now firewalld
systemctl enable --now httpd
```

### TO change apache port

```
vi /etc/httpd/conf/httpd.conf
```

### and replace port

```
Listen 80 -> Listen 8080
```

or

```
sed -i 's/Listen 80/Listen 8080/g' /etc/httpd/conf/httpd.conf
```

### Add Apache port to firewall:

```
firewall-cmd --zone=public --add-port=8080/tcp --permanent
firewall-cmd --reload

```

### Manage SELinux to add new Apache port:

```
semanage port -a -t http_port_t -p tcp 8080
```

This command adds port `8080` to the list of ports allowed for the `http_port_t` SELinux type.

### Set SELinux boolean httpd_can_network_connect_db to on:

SELinux boolean values are settings that control various aspects of SELinux behavior. For example, the httpd_can_network_connect boolean controls whether the Apache HTTP Server is allowed to make network connections.

Here's an example command that sets the httpd_can_network_connect boolean to on:

```
sebool -P httpd_can_network_connect on
```

The -P option makes the change persistent, so that it will survive a system reboot.

### Restart Apache

```
sudo systemctl daemon-reload
sudo systemctl restart httpd.service
```

to show Apache pages on `<ip_server>:8080`

## MySQL Server

### Install MySQL-Server on RHEL & CentOS Stream 9

```
sudo yum update -y
sudo install mysql-server
```

and start mysql

```
sudo systemctl enable --now mysqld
```

in real you don`t this beacuase mysql is running after install automatically

### Set MySQL root password

```
sudo mysql_secure_installation
```

create your root password

and then using the root password when login in MySQl

```
sudo mysql -u root -p
```

### Create database

```
CREATE DATABASE database_name;
```

replace `database_name` to any name you want

### Create user and grant privileges

```
CREATE USER 'new_user'@'%' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON new_database.* TO 'new_user'@'localhost' IDENTIFIED BY 'password';
flush privileges
```

Replace new_user and password with the desired username and password, respectively.

### CREATE TABLE

```
USE database_name;
```

Replace `database_name` with the name of the database you want to use.

```
CREATE TABLE table_name (
   column1 datatype,
   column2 datatype,
   column3 datatype,
   ...
);
```

Replace `table_name` with the desired name for your table, and define the columns and their datatypes inside the parentheses.

For example, to create a table named `customers` with columns for `id`, `name`, and `email`, you can run the following command:
