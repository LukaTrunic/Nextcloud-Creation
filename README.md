# ☁️ Self-Hosted Nextcloud on Oracle VM VirtualBox (Ubuntu 22.04.3)

This project documents the successful installation and configuration of **Nextcloud**, an open-source file sharing and collaboration platform, using **Oracle VM VirtualBox** running **Ubuntu 22.04.3 LTS**. The deployment stack includes **Apache2**, **MariaDB**, and **PHP**, forming a full LAMP-like environment for self-hosted cloud storage.


## 📦 Technologies Used

- Oracle VM VirtualBox
- Ubuntu 22.04.3 LTS (Guest OS)
- Apache2 (Web server)
- MariaDB (Database)
- PHP 8.x (Runtime)
- Nextcloud (Cloud file system)


## ⚙️ Installation Steps

### 1. Set Up the VM
- Download Ubuntu 22.04.3 ISO
- Create a new VM in Oracle VirtualBox
- Install Ubuntu and update packages:
  ```bash
  sudo apt update && sudo apt upgrade
  ```

### 2. Install Apache2
```bash
sudo apt install apache2
sudo systemctl enable apache2
sudo systemctl start apache2
```

### 3. Install MariaDB
```bash
sudo apt install mariadb-server
sudo systemctl enable mariadb
sudo systemctl start mariadb
```

**Create the Nextcloud database and user:**
```bash
sudo mysql -u root -p
```
Then inside MySQL shell:
```sql
CREATE DATABASE nextcloud;
CREATE USER 'nextcloud'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON nextcloud.* TO 'nextcloud'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

### 4. Install PHP
```bash
sudo apt install php php-mysql libapache2-mod-php php-xml php-gd php-curl php-zip php-mbstring
```


## 📥 Install Nextcloud

1. **Download and extract Nextcloud:**
   ```bash
   wget https://download.nextcloud.com/server/releases/latest.tar.bz2
   tar -xjf latest.tar.bz2
   sudo mv nextcloud /var/www/html/
   ```

2. **Set permissions:**
   ```bash
   sudo chown -R www-data:www-data /var/www/html/nextcloud
   sudo chmod -R 755 /var/www/html/nextcloud
   ```


## 🌐 Apache Configuration

1. **Create config file:**
   ```bash
   sudo nano /etc/apache2/sites-available/nextcloud.conf
   ```
   Add the following:
   ```apache
   Alias /nextcloud "/var/www/html/nextcloud"
   <Directory /var/www/html/nextcloud>
       Options +FollowSymlinks
       AllowOverride All
       Require all granted
       <IfModule mod_dav.c>
           Dav off
       </IfModule>
       SetEnv HOME /var/www/html/nextcloud
       SetEnv HTTP_HOME /var/www/html/nextcloud
   </Directory>
   ```

2. **Enable the config and rewrite module:**
   ```bash
   sudo a2ensite nextcloud.conf
   sudo a2enmod rewrite
   sudo systemctl reload apache2
   ```


## 🚀 Access the Web Interface

- Get your VM's IP address:
  ```bash
  ip a
  ```
- Visit: `http://<your-ip-address>/nextcloud`

Use the browser setup screen to create an admin account and connect to the MariaDB instance.


## ✅ Summary

This project demonstrates a full deployment of a self-hosted Nextcloud server using a Linux VM. The platform can now be used for private cloud storage and collaboration, with support for file uploads, syncing, and user management — all hosted securely on a local VM environment.


## 📬 Author

Luka Trunić
