# 🛠️ WP-Deploy: WordPress Deployment & Database Management Scripts

WP-Deploy is a lightweight and efficient deployment tool for WordPress sites using Docker. It provides automated database backups, restores, and deployment workflows for syncing local and remote environments.

## ⚙️ Features
- **Automated Deployments** → Deploy WordPress updates to a remote server.
- **Database Backup & Restore** → Securely backup and restore the database.
- **Global CLI Access** → Install scripts system-wide for easy execution.
- **Configurable Settings** → Uses `wp-deploy.ini` for environment-specific settings.
- **Safe File Synchronization** → Uses `rsync` for efficient file transfers.
- **Modular Structure** → Scripts can be used independently or together.

---

## 📂 Directory Structure

```
wp-deploy/
 ├── wp-db-backup         # Script to create a database backup
 ├── wp-db-restore        # Script to restore a database backup
 ├── wp-deploy            # Script to deploy changes to a remote server
 ├── wp-pull-from-remote  # Script to sync production data locally
 ├── wp-functions         # Shared functions for deployment and database management
 ├── install              # Script to install wp-deploy commands globally
 ├── wp-deploy.ini.sample # Sample configuration file (copy to wp-deploy.ini)
 ├── README.md            # Documentation
```

---

## ⚙️ Setup & Installation

### 📥 Clone the Repository
```bash
git clone git@github.com:emada/wp-deploy.git
cd wp-deploy
```

### 🔧 Run the Installer
```bash
./install
```
This will:
✅ Clone `wp-deploy` to your system.
✅ Create symbolic links (`ln -s`) in `/usr/local/bin/`.
✅ Make all scripts accessible globally.

---

## ⚙️ Configuration (`wp-deploy.ini`)
Before using `wp-deploy`, copy the sample configuration to the root directory of your Wordpress docker project and modify it as needed:
```bash
cp wp-deploy.ini.sample <WP_PROJEC_ROOT>/wp-deploy.ini
```
Edit `<WP_PROJEC_ROOT>/wp-deploy.ini` with your WordPress environment settings:
```ini
DB_CONTAINER_NAME="db"       # Docker container running MySQL/MariaDB
DB_USER="wp"                 # Database user
DB_PASSWORD="12345"          # Database password
DB_NAME="wpdb"               # Database name
BACKUP_DIR="./wp-backups"    # Where backups are stored
```

---

## 📌 Usage
Ensure you run the wp-deploy commands from within the WordPress root directory. The expected directory structure is:
```
wordpress-project-root/
 ├── wp-backups           # It'll be created if it doesn't exist
 ├── docs                 # Miscellaneous files unrelated to the deployment process
 ├── www                  # Root directory for the WordPress project
 │   ├── html             # Contains WordPress core and project files
 ├── docker-compose.yml   # Docker Compose configuration for managing containers
 ├── wp-deploy.ini        # Configuration file for WP-Deploy settings
```

### 🚀 Deployment
Deploy the project to the remote server:
```bash
wp-deploy theway
```

First Deployment:
✅ Copy all project files to the remote server.
✅ Upload and restore the local database on the remote server.
✅ Start Docker Compose remotely.

Subsequent Deployments:
✅ Backup the existing project files and database.
✅ Sync only modified files to the remote server.
✅ Upload and restore the local database on the remote server.
✅ Restart Docker Compose remotely if needed.

---

### ⬇️ Pull the Latest Data from Remote Server
Sync the latest production data locally:
```bash
wp-pull-from-remote theway
```
This will:
✅ Download the latest database dump.
✅ Restore it to your local environment.
✅ Sync the whole Wordpress `html` directory.

---

## 📌 Manually handling the database

### 🔄 Create a Database Backup
```bash
wp-db-backup
```
- Saves a `.sql` file to `./wp-backups/` with a **timestamp**.

---

### 💾 Restore a Database Backup
Restore the **latest** backup:
```bash
wp-db-restore
```
Restore a **specific backup file**:
```bash
wp-db-restore wp-backups/db_backup_YYYY-MM-DD_HH-MM-SS.sql
```
- **⚠️ WARNING:** The restore script will prompt for confirmation before overwriting the database.

---

## 🔥 Why Use WP-Deploy?
✅ **No Hardcoded Credentials** → All settings are in `wp-deploy.ini`.
✅ **Minimal Risk** → Prevents accidental overwrites with confirmation prompts.
✅ **Modular & Reusable** → Functions are centralized in `wp-functions`.
✅ **Works with Docker** → No need to install MySQL locally.
✅ **Quick & Reliable** → Uses `rsync` for fast, incremental file transfers.

---

## 🚀 Future Improvements
- [ ] Automatic scheduled backups via `cron`.
- [ ] Support for remote backup storage (AWS S3, Google Drive).
- [ ] Encrypt backups for added security.

---

## 📝 Author
Maintained by **Emerson**.

For issues or feature requests, feel free to **open an issue or contribute**! 🚀

