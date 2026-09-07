# 📖 Detailed Enterprise WordPress Setup Guide (Ubuntu 26.04.1 LTS)

This guide documents the technical implementation of a high-performance
WordPress environment on AWS EC2 using a modern LEMP stack (Linux,
Nginx, MySQL, PHP 8.5) and Redis Object Caching.

------------------------------------------------------------------------

## 🛠️ Phase 1: LEMP Stack Installation

WordPress requires a web server, a database engine, and a PHP processor.
In this project, we used **Nginx** and **PHP-FPM** for superior
performance compared to traditional Apache setups.

### 1. Install Core Components

``` bash
sudo apt update
sudo apt install nginx mysql-server php8.5-fpm php-mysql php-redis redis-server -y
```

### 2. Verify PHP-FPM Status

Since Nginx does not process PHP directly, we use the FastCGI Process
Manager (FPM).

``` bash
sudo systemctl status php8.5-fpm
```

------------------------------------------------------------------------

## 🛠️ Phase 2: Database Hardening & Configuration

WordPress saves all posts, users, and settings in a MySQL database. We
avoid using the `root` user for security.

### 1. Create Dedicated DB and User

``` sql
-- Login to MySQL: sudo mysql
CREATE DATABASE wordpress_db DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
CREATE USER 'wp_admin'@'localhost' IDENTIFIED BY 'YOUR_STRONG_PASSWORD';
GRANT ALL PRIVILEGES ON wordpress_db.* TO 'wp_admin'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

------------------------------------------------------------------------

## 🛠️ Phase 3: WordPress Deployment & Permissions

Proper file permissions are the first line of defense against
unauthorized code execution in WordPress.

### 1. Download and Extract

``` bash
cd /tmp
wget https://wordpress.org/latest.tar.gz
tar xzvf latest.tar.gz
sudo mkdir -p /var/www/wordpress
sudo cp -a /tmp/wordpress/. /var/www/wordpress/
```

### 2. Set Secure Ownership and Permissions

We assign ownership to `www-data` (the Nginx user) and restrict file
access:

``` bash
sudo chown -R www-data:www-data /var/www/wordpress
sudo find /var/www/wordpress/ -type d -exec chmod 750 {} \;
sudo find /var/www/wordpress/ -type f -exec chmod 640 {} \;
```

------------------------------------------------------------------------

## 🛠️ Phase 4: Performance Optimization (Redis)

To minimize Database latency, we implemented **Redis Object Caching**.
This stores database queries in RAM (faster than Disk).

1.  **Verify Redis:** Run `redis-cli ping`. It should return `PONG`.
2.  **Monitor Traffic:** Use `redis-cli monitor` while refreshing the
    site to see real-time cache activity.

------------------------------------------------------------------------

## 🛠️ Phase 5: SSL/TLS Encryption

End-to-end encryption was implemented using **Let's Encrypt** to protect
user data and improve SEO rankings.

``` bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d vsdapp.duckdns.org
```

------------------------------------------------------------------------

## ⚠️ Troubleshooting & Resolutions (Real-World Debugging)

### 1. Issue: 502 Bad Gateway

-   **Root Cause:** Nginx was configured to look for a PHP 8.3 socket,
    but the system had **PHP 8.5** installed.

-   **Resolution:** Updated the Nginx `fastcgi_pass` directive to point
    to the correct socket path:

    `/var/run/php/php8.5-fpm.sock`

### 2. Issue: Port 80 "Address already in use"

-   **Root Cause:** Apache2 was pre-installed or auto-started,
    preventing Nginx from binding to Port 80.
-   **Resolution:**

``` bash
sudo systemctl stop apache2
sudo systemctl disable apache2
sudo systemctl restart nginx
```

### 3. Issue: Redis Cache Failure

-   **Root Cause:** PHP-FPM lacked write permissions to the
    `/wp-content` directory to create the required `object-cache.php`
    drop-in.
-   **Resolution:** Corrected folder ownership using
    `chown -R www-data:www-data` and adjusted permissions to `775`
    temporarily for the initial setup.
