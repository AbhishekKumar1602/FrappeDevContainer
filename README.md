# Frappe VSCode Dev Container Setup
This repository provides a **VS Code Dev Container** setup for developing Frappe applications using **Docker**. It supports both **[New Projects](https://github.com/AbhishekKumar1602/FrappeDevContainer?tab=readme-ov-file#new-frappe-project-vs-code-dev-container-setup-guide)** and **[Existing Projects](https://github.com/AbhishekKumar1602/FrappeDevContainer?tab=readme-ov-file#existing-frappe-project-vs-code-dev-container-setup-guide)**.

## Requirements
* VS Code
* Docker
* Git
* MariaDB/PostgreSQL Database With Root User and Password.  

**NOTE:** *If needed refer **[MariaDB](https://github.com/AbhishekKumar1602/FrappeDevContainer?tab=readme-ov-file#steps-to-install-and-configure-mariadb-for-development)** and **[PostgrSQL](https://github.com/AbhishekKumar1602/FrappeDevContainer?tab=readme-ov-file#steps-to-install-and-configure-mariadb-for-development)** Setup Guide.*

---

## New Frappe Project VS Code Dev Container Setup Guide

### **Step 1: Clone the Dev Container Repository**

```bash
git clone ssh://git@gitlab.expediensolutions.in:9865/frappe/frappe-vscode-dev-container-setup.git
echo -n "Enter New Directory Name: "
read new_dir
mv frappe-vscode-dev-container-setup "$new_dir"
cd "$new_dir"
```

### **Step 2: Remove Old Git Configuration**

```bash
rm -rf .git*
```

### **Step 3: Setup Devcontainer Config**

```bash
cp -R devcontainer-example .devcontainer
```

### **Step 4: Setup VS Code Configs**

```bash
cp -R development/vscode-example development/.vscode
```

### **Step 5: Install VS Code Extension**

Install the **Remote Containers** extension:

```bash
code --install-extension ms-vscode-remote.remote-containers
```

### **Step 6: Open the Project in Dev Container**

```bash
code .
```

1. Press **Ctrl + Shift + P**
2. Search for **Remote-Containers: Reopen in Container**
3. VS Code will now build and start the container environment.

### **Step 7: Initialize Frappe Bench**

```bash
PYENV_VERSION=3.10.13 bench init --skip-redis-config-generation --frappe-branch version-15 frappe-bench
cd frappe-bench
```

### **Step 8: Configure Redis Services**

```bash
bench set-config -g redis_cache redis://redis-cache:6379
bench set-config -g redis_queue redis://redis-queue:6379
bench set-config -g redis_socketio redis://redis-queue:6379
```

### **Step 9: Create a New Site**

```bash
bench new-site mysite.localhost \
  --db-host 192.168.10.20 \
  --db-port 3306 \
  --db-type mariadb \
  --db-root-username root \
  --db-root-password Root1234 \
  --db-name test \
  --db-password Test1234 \
  --admin-password Admin12345
```

### **Step 10: Set the Default Site**

```bash
bench use mysite.localhost
```

### **Step 11: Enable Developer Mode**

```bash
bench --site mysite.localhost set-config developer_mode 1
bench --site mysite.localhost clear-cache
```

### **Step 12: (Optional) Install Frappe Apps like ERPNext**

```bash
bench get-app --branch version-15 erpnext
bench --site mysite.localhost install-app erpnext
```

### **Step 13: Build & Migrate**

```bash
bench build
bench --site mysite.localhost migrate
```

### **Step 14: Start the Server**

```bash
bench start
```

### **Step 15: Access Your Application At:**

```
http://mysite.localhost:8000
```

### **Next: Build Custom Apps**

Example:

```bash
bench new-app my_custom_app
bench --site mysite.localhost install-app my_custom_app
```

---

## Existing Frappe Project VS Code Dev Container Setup Guide

### **Step 1: Clone the Dev Container Repository**

```bash
git clone ssh://git@gitlab.expediensolutions.in:9865/frappe/frappe-vscode-dev-container-setup.git
echo -n "Enter New Directory Name: "
read new_dir
mv frappe-vscode-dev-container-setup "$new_dir"
cd "$new_dir"
```

### **Step 2: Remove Old Git Configuration**

```bash
rm -rf .git*
```

### **Step 3: Setup Devcontainer Config**

```bash
cp -R devcontainer-example .devcontainer
```

### **Step 4: Setup VS Code Configs**

```bash
cp -R development/vscode-example development/.vscode
```

### **Step 5: Install VS Code Extension**

Install the **Remote Containers** extension:

```bash
code --install-extension ms-vscode-remote.remote-containers
```

### **Step 6: Open the Project in Dev Container**

```bash
code .
```

1. Press **Ctrl + Shift + P**
2. Search for **Remote-Containers: Reopen in Container**
3. VS Code will build and start the container environment.

### **Step 7: Initialize Frappe Bench**

```bash
PYENV_VERSION=3.10.13 bench init --skip-redis-config-generation --frappe-branch version-15 frappe-bench
cd frappe-bench
```

### **Step 8: Configure Redis Services**

```bash
bench set-config -g redis_cache redis://redis-cache:6379
bench set-config -g redis_queue redis://redis-queue:6379
bench set-config -g redis_socketio redis://redis-queue:6379
```

### **Step 9: Create Site Directory for Your Existing Site**

```bash
mkdir -p sites/mysite.localhost/logs
touch sites/mysite.localhost/site_config.json
install -m 644 /dev/null sites/mysite.localhost/logs/database.log
chown frappe:frappe sites/mysite.localhost/logs/database.log
```

### **Step 10: Add Existing Site Config**

```json
{
  "db_host": "<database_host_ip>",
  "db_port": <database_host_port>,
  "db_type": "<database_engine_name>",
  "db_name": "<database_name>",
  "db_password": "<database_password>",
  "developer_mode": 1
}
```

### **Step 11: Install All Applications for the Site**

```bash
bench get-app --branch <branch_name> <remote_repository_url>
bench --site mysite.localhost install-app <app_name>
```

### **Step 12: Build & Migrate**

```bash
bench build
bench --site mysite.localhost migrate
```

### **Step 13: Start the Server**

```bash
bench start
```

### **Step 14: Access Your Application At:**

```
http://mysite.localhost:8000
```
---

## Steps to Install and Configure MariaDB For Development
 
### **Step 1: Update Your Package List**

```bash
sudo apt update
```

### **Step 2: Install MariaDB Server**

```bash
sudo apt install mariadb-server
```

### **Step 3: Start and Enable MariaDB**

```bash
sudo systemctl start mariadb
sudo systemctl enable mariadb
```

### **Step 4: Check MariaDB Status**

```bash
sudo systemctl status mariadb
```

### **Step 5: Run the MariaDB Secure Installation**

```bash
sudo mysql_secure_installation
```

* **Follow the Prompts:**
```
  * Enter current root password (enter if none)
  * Set root password?  Y
  * Remove anonymous users? Y
  * Disallow root login remotely?  N
  * Remove test DB? Y
  * Reload privilege tables? Y
```

### **Step 6: Login to MariaDB as Root User**

```bash
sudo mysql -u root -p
```
* Enter The Password You Entered In Prompt


### **Step 7: Allow MariaDB to Listen On All Interfaces**

```bash
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```
**Find:** bind-address = 127.0.0.1
**Change To:** bind-address = 0.0.0.0
**Save and Exit**

### **Step 8: Restart the Service**

```bash
sudo systemctl restart mariadb
```

### **Step 9: Create/Enable `root` for Remote Access and Grant Privileges**

* Login as the `mysql` system user:

```bash
sudo mysql -u root -p
```
* Enter the Password You Entered at Secure Setup

* Run This SQL Query(Replace `Root@9223#` with Your Password):
```sql
CREATE USER IF NOT EXISTS 'root'@'%' IDENTIFIED BY 'Root@9223#';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
ALTER USER 'root'@'localhost' IDENTIFIED BY 'Root@9223#';
FLUSH PRIVILEGES;
EXIT;
```

### **Step 10: Configure Firewall**

**Allow Only an IP**
```bash
sudo ufw allow from 203.0.113.10 to any port 3306 proto tcp
```
**Allow a Network**
```bash
sudo ufw allow from 203.0.113.0/24 to any port 3306 proto tcp
```
**Allow All**
```bash
sudo ufw allow 3306/tcp
```

### **Step 11: Test Remote Connection from a Client Machine**

```bash
mysql -h <server-ip-or-domain> -u root -p
```
**Enter Password Root@9223#**

---

## Steps to Install and Configure PostgreSQL For Development

### **Step 1: Update Your Package List**

```bash
sudo apt update
```

### **Step 2 Install PostgreSQL Server**

```bash
sudo apt install postgresql postgresql-contrib
```

### **Step 3 Start and Enable PostgreSQL**

```bash
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

### **Step 4: Check PostgreSQL Status**

```bash
sudo systemctl status postgresql
```

### **Step 5: Allow PostgreSQL to Listen on All Interfaces**

```bash
sudo nano /etc/postgresql/<version>/main/postgresql.conf
```

**Find and Change:**

```
#listen_addresses = 'localhost'
```

**To:**

```
listen_addresses = '*'
```

**Save and Exit.**

### **Step 6: Allow Remote Connections in `pg_hba.conf`**

**Edit:**

```bash
sudo nano /etc/postgresql/15/main/pg_hba.conf
```

**Add this line at the bottom (replace `0.0.0.0/0` with your IP/network if needed):**

```
host    all             all             0.0.0.0/0               md5
```

**Save and Exit.**

### **Step 7: Restart the Service**

```bash
sudo systemctl restart postgresql
```

### **Step 8: Create/Enable `postgres` Superuser with Password**

* Login as the `postgres` system user:

```bash
sudo -u postgres psql
```

* Inside PostgreSQL shell, set a password (replace `Root@9223#` with your own):

```sql
ALTER USER postgres WITH PASSWORD 'Root@9223#';
\q
```

### **Step 9: Configure Firewall**

**Allow Only an IP**

```bash
sudo ufw allow from 203.0.113.10 to any port 5432 proto tcp
```

**Allow a Network**

```bash
sudo ufw allow from 203.0.113.0/24 to any port 5432 proto tcp
```

**Allow All**

```bash
sudo ufw allow 5432/tcp
```

### **Step 10: Test Remote Connection from a Client Machine**

```bash
psql -h <server-ip-or-domain> -U postgres -d postgres
```

**Enter Password: `Root@9223#`**
