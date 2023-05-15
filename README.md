# Configure_Apache_MySQL_server

### Install and Configure Apache Server and connect MySQL Server

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
