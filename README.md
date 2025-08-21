# Frappe Dev Container
"This repository provides a complete guide to setting up and running Frappe inside a VS Code Dev Container. Whether you're starting a [New Project](https://github.com/AbhishekKumar1602/FrappeDevContainer?tab=readme-ov-file#new-frappe-project-vs-code-dev-container-setup-guide)  or working with an [Existing Project](https://github.com/AbhishekKumar1602/FrappeDevContainer?tab=readme-ov-file#existing-frappe-project-vs-code-dev-container-setup-guide), this setup ensures a consistent, reproducible, and developer-friendly environment."

## Requirements
* VS Code
* Docker
* Git
* MariaDB/PostgreSQL Database With Root User and Password

### Note 
Update **Port Number** in `.devcontainer/docker-compose.yml` while running multiple DevContainers on same machine.

---

## New Frappe Project VS Code Dev Container Setup Guide

### **Step 1: Clone the Dev Container Repository**

```bash
git clone https://github.com/AbhishekKumar1602/FrappeDevContainer.git
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

### **Step 5: Install VS Code Remote Containers Extension**

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
  --db-host <database_host_ip> \
  --db-port <database_host_port> \
  --db-type <database_engine_name> \
  --db-root-username <database_root_user_name> \
  --db-root-password <database_root_user_password> \
  --db-name <new_database_name> \
  --db-password <new_database_password> \
  --admin-password <admin_password>
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
git clone https://github.com/AbhishekKumar1602/FrappeDevContainer.git
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

### **Step 5: Install VS Code Remote Containers Extension**

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

### **Step 11: Set the Default Site**

```bash
bench use mysite.localhost
```

### **Step 12: Install All Applications for the Site**

```bash
bench get-app --branch <branch_name> <remote_repository_url>
bench --site mysite.localhost install-app <app_name>
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