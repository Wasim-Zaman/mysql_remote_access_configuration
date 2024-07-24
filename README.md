# MySQL Remote Access Configuration Guide

This guide covers the steps to configure MySQL for remote access and connect to it using MySQL Workbench or Node.js with Prisma. 

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Configure MySQL for Remote Access](#configure-mysql-for-remote-access)
3. [Grant Remote Access to MySQL User](#grant-remote-access-to-mysql-user)
4. [Firewall Configuration](#firewall-configuration)
5. [Connect Using MySQL Workbench](#connect-using-mysql-workbench)
6. [Connect Using Node.js and Prisma](#connect-using-nodejs-and-prisma)
7. [Troubleshooting](#troubleshooting)

## Prerequisites

- A VPS running MySQL (version 8.0 or higher)
- MySQL installed on the VPS
- Access to the VPS terminal
- MySQL Workbench installed on your local machine
- Node.js installed on your local machine

## Configure MySQL for Remote Access

1. **Edit MySQL Configuration File:**

   Open the MySQL configuration file (`/etc/mysql/mysql.conf.d/mysqld.cnf` or `/etc/my.cnf`):

   ```sh
   sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
   ```
## Update the bind-address directive to allow connections from any IP address:

```ssh
[mysqld]
bind-address = 0.0.0.0
```
## Restart MySQL Service:

Apply the changes by restarting the MySQL service:
```sh
sudo systemctl restart mysql
```

## Grant Remote Access to MySQL User
- Create a user with remote access and grant privileges:

```ssh
CREATE USER 'root'@'%' IDENTIFIED BY 'your-password';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

- Verify the User:
```ssh
SELECT host, user FROM mysql.user WHERE user = 'root';
```

- You should see:
```ssh
+-----------+------+
| host      | user |
+-----------+------+
| %         | root |
| localhost | root |
+-----------+------+
```

## Firewall Configuration

Ensure that your VPS firewall allows incoming connections on port 3306.

For UFW (Uncomplicated Firewall):

```ssh
sudo ufw allow 3306
sudo ufw reload
```

```ssh
sudo iptables -A INPUT -p tcp --dport 3306 -j ACCEPT
```

