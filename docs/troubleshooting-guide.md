# 🛠 Troubleshooting Guide: WordPress on LEMP Stack

### 1. Issue: 502 Bad Gateway Error
- **Symptoms:** Nginx was running, but the site returned a 502 error.
- **Root Cause:** Mismatch between Nginx configuration and the PHP-FPM socket path.
- **Resolution:** Verified the PHP version (8.5) and updated the `fastcgi_pass` directive in Nginx to point to the correct socket: `/var/run/php/php8.5-fpm.sock`.

### 2. Issue: Port 80 "Address already in use"
- **Symptoms:** Nginx failed to start.
- **Root Cause:** Apache (installed by default with some packages) had captured Port 80.
- **Resolution:** Used `lsof -i :80` to identify the process, then stopped and disabled Apache: `sudo systemctl stop apache2 && sudo systemctl disable apache2`.

### 3. Issue: Redis Option Not Showing in Dashboard
- **Symptoms:** The Redis plugin was active but couldn't write the object-cache file.
- **Resolution:** Fixed Linux file permissions. Changed owner to `www-data` and set directory permissions to `775` to allow the plugin to create `object-cache.php`.
