# Configure Apache MySQL Server using Ansible

This repository contains a collection of Ansible playbooks that can be used to configure an Apache MySQL server. The playbooks automate the installation of MySQL and Apache, as well as the configuration of Apache's port.

## Requirements

To use these playbooks, you'll need to have the following software installed on your local machine:

- Ansible
- Git

You'll also need to have SSH access to the server that you want to configure.

## Usage

To use these playbooks, you'll need to have Ansible installed on your local machine. There are different ways to install Ansible:

### Installing Ansible using yum package manager in RHEL

You can use the system package manager to install Ansible. Here are the instructions for installing Ansible on RHEL:

```
sudo yum install epel-release
sudo yum install ansible
ansible --version
```

### Installing Ansible using pip

You can install Ansible using pip, the Python package manager. First, you need to install pip if it's not already installed. You can do that using the following command:

````
sudo apt-get update
sudo apt-get install python3-pip
sudo pip3 install ansible
ansible --version
```

### Frok this repo

Fork the repository by clicking the "Fork" button on the top right corner of this page. This will create a copy of the repository under your GitHub account.

### Clon this repo

Clone the forked repository to your local machine using the following command:

```
git clone https://github.com/<your-username>/Configure_Apache_MySQL_server.git
```

Replace `your-username` with your GitHub username.

### Change directory

1. Change into the cloned repository directory:
```
cd Configure_Apache_MySQL_server
```

2. Update the `inventory` file with the IP address .

3. Update the `inventory` file with the private key.

4. Update the `vars/main.yml` file with the desired MySQL root password and Apache port number.

5. Run the following command to execute the playbook:

```
ansible-playbook -i inventory playbook.yml
```

# Configure_Apache_MySQL_server Manually

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
GRANT ALL PRIVILEGES ON new_database.\* TO 'new_user'@'%' IDENTIFIED BY 'password';
flush privileges
```

Replace new_user and password with the desired username and password, respectively.
Replace `%` to `IP apache server ` more secure

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

```
CREATE TABLE customers (
id INT NOT NULL AUTO_INCREMENT,
name VARCHAR(50) NOT NULL,
email VARCHAR(255) NOT NULL,
PRIMARY KEY (id)
);
```

This creates a table with three columns: `id`, `name`, and `email`. The `id` column is an auto-incrementing integer that serves as the primary key for the table.


````
