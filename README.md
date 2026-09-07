# 🚀 Enterprise WordPress Hosting on AWS (LEMP Stack)

**Objective:** Deploy a highly optimized, production-grade WordPress environment on AWS EC2 using Nginx, PHP 8.5-FPM, and Redis Object Caching.

---

### 📊 Performance & Verification Proof

#### 1. Real-time Redis Activity (Database Optimization)
Demonstrating live database query offloading to RAM (Redis) for sub-second response times.
<img width="1918" height="1052" alt="Screenshot from 2026-09-07 12-01-47" src="https://github.com/user-attachments/assets/1da4e220-ec4d-4a10-9e00-c46ccedced9c" />


#### 2. Redis Integration Status
Verified backend connectivity between the PHP engine and the Redis server.
<img width="1917" height="1052" alt="Screenshot from 2026-09-07 12-02-47" src="https://github.com/user-attachments/assets/0d30b3d0-ee63-4577-a96a-21c4616e6c70" />


#### 3. WordPress Security & Site Health
Ensuring 100% compliance with WordPress security benchmarks and performance health checks.
<img width="1920" height="1046" alt="Screenshot from 2026-09-07 11-59-32" src="https://github.com/user-attachments/assets/63200496-2a29-4119-afe0-2c5e09ebcc7b" />


#### 4. High-Performance Nginx & PHP-FPM Setup
Verified configuration for optimized PHP-FPM socket communication.
<img width="1919" height="1051" alt="Screenshot from 2026-09-07 12-03-29" src="https://github.com/user-attachments/assets/692cc109-93ec-4b10-85a6-563e6b3f3e86" />


#### 5. Final Hardened Environment (Verified SSL)
Production environment secured with Let's Encrypt SSL/TLS and modern UI.
<img width="1919" height="1051" alt="Screenshot from 2026-09-07 12-04-52" src="https://github.com/user-attachments/assets/fb653a9d-efa1-4c48-ba9c-e89043cf6f43" />


---

### 📁 Technical Breakdown
*   **/configs**: Nginx server blocks and optimized PHP-FPM settings.
*   **/docs**: [Step-by-Step Troubleshooting Guide](docs/troubleshooting-guide.md) (Port conflicts & Socket management).

### 🛠️ Tech Stack
- **OS:** Ubuntu 26.04.1 LTS
- **Web Server:** Nginx (PHP-FPM 8.5)
- **Database:** MySQL 8.0+
- **Cache:** Redis Server (Object Cache)
- **Security:** Certbot (SSL), UFW, Hardened File Permissions (750/640)
