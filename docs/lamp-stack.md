# Installing the LAMP Stack with MariaDB on Ubuntu/Linux Mint

**LAMP** stands for **L**inux, **A**pache, **M**ariaDB, and **P**HP. This tutorial walks you through installing and configuring a full LAMP stack on Ubuntu or Linux Mint (22.x).

---

## Prerequisites

- A system running Ubuntu 22.04+ or Linux Mint 22.x
- A user account with `sudo` privileges
- An internet connection

---

## Step 1: Update Your System

Before installing anything, make sure your package list and installed packages are up to date.

```bash
sudo apt update && sudo apt upgrade -y
```

---

## Step 2: Install Apache

Apache is the web server that will serve your web content.

```bash
sudo apt install apache2 -y
```

### Verify Apache is running

```bash
sudo systemctl status apache2
```

You should see `active (running)` in the output. You can also open a browser and navigate to `http://127.0.0.1/` — you should see the default Apache welcome page.

### Enable Apache to start on boot

```bash
sudo systemctl enable apache2
```

---

## Step 3: Install MariaDB

MariaDB is a community-developed fork of MySQL and is fully compatible with it.

```bash
sudo apt install mariadb-server mariadb-client -y
```

### Start and enable MariaDB

```bash
sudo systemctl start mariadb
sudo systemctl enable mariadb
```

### Secure the MariaDB installation

Run the included security script to set a root password, remove anonymous users, and disable remote root login.

```bash
sudo mysql_secure_installation
```

You will be prompted with a series of questions. Recommended answers:

| Prompt | Recommended Answer |
|---|---|
| Switch to unix_socket authentication? | `N` |
| Change the root password? | `Y` (set a strong password) |
| Remove anonymous users? | `Y` |
| Disallow root login remotely? | `Y` |
| Remove test database? | `Y` |
| Reload privilege tables? | `Y` |

### Verify MariaDB is running

```bash
sudo systemctl status mariadb
```

### Log in to MariaDB

```bash
sudo mariadb -u root -p
```

Enter your root password when prompted. Once inside, you can exit with:

```sql
EXIT;
```

---

## Step 4: Install PHP

PHP is the server-side scripting language that connects Apache and MariaDB together.

```bash
sudo apt install php libapache2-mod-php php-mysql -y
```

### Useful additional PHP extensions

These are commonly needed for web applications like WordPress, Joomla, etc.:

```bash
sudo apt install php-curl php-gd php-mbstring php-xml php-zip php-intl php-bcmath -y
```

### Verify PHP is installed

```bash
php -v
```

---

## Step 5: Configure Apache to Prefer PHP Files

By default, Apache serves `index.html` before `index.php`. To change this:

```bash
sudo subl /etc/apache2/mods-enabled/dir.conf
```

Find the `DirectoryIndex` line and move `index.php` to the front:

```apache
DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
```

Save the file, then restart Apache:

```bash
sudo systemctl restart apache2
```

---

## Step 6: Test PHP

Create a test PHP file in the web root:

```bash
sudo subl /var/www/html/info.php
```

Add the following content:

```php
<?php
phpinfo();
?>
```

Save the file and open `http://127.0.0.1/info.php` in your browser. You should see a detailed PHP information page.

> ⚠️ **Security note:** Delete this file once you've confirmed PHP is working.

```bash
sudo rm /var/www/html/info.php
```

---

## Step 7: Create a Database and User (Optional but Recommended)

Rather than using the root account for your applications, create a dedicated database and user.

```bash
sudo mariadb -u root -p
```

Inside the MariaDB shell:

```sql
CREATE DATABASE myapp_db;
CREATE USER 'myapp_user'@'localhost' IDENTIFIED BY 'your_strong_password';
GRANT ALL PRIVILEGES ON myapp_db.* TO 'myapp_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

---

## Step 8: Configure Apache Virtual Hosts (Optional)

If you plan to run multiple sites, set up a virtual host for each one. Your web files live in `/var/www/html/`.

Create a new virtual host config:

```bash
sudo subl /etc/apache2/sites-available/mysite.conf
```

Add the following (adjust as needed):

```apache
<VirtualHost *:80>
    ServerName mysite.local
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html/mysite

    <Directory /var/www/html/mysite>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/mysite_error.log
    CustomLog ${APACHE_LOG_DIR}/mysite_access.log combined
</VirtualHost>
```

Enable the site and the `rewrite` module, then reload Apache:

```bash
sudo a2ensite mysite.conf
sudo a2enmod rewrite
sudo systemctl reload apache2
```

---

## Summary

| Component | Package | Service |
|---|---|---|
| Web Server | `apache2` | `apache2` |
| Database | `mariadb-server` | `mariadb` |
| PHP | `php libapache2-mod-php php-mysql` | *(handled by Apache)* |

Your LAMP stack is now installed and ready to use. Web files go in `/var/www/html/`, and you can manage your databases via `sudo mariadb -u root -p`.

---

## Quick Reference: Useful Commands

```bash
# Apache
sudo systemctl start|stop|restart|reload|status apache2

# MariaDB
sudo systemctl start|stop|restart|status mariadb
sudo mariadb -u root -p

# Check Apache config for errors
sudo apache2ctl configtest

# Enable/disable Apache modules
sudo a2enmod <module>
sudo a2dismod <module>

# Enable/disable Apache sites
sudo a2ensite <site>.conf
sudo a2dissite <site>.conf
```
