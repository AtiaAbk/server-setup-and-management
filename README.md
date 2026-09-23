# 🖥️ Hosting a Website on Your Own Server (LAMP Stack)

<div align="center">

![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![Apache](https://img.shields.io/badge/Apache-D22128?style=for-the-badge&logo=apache&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![PHP](https://img.shields.io/badge/PHP-777BB4?style=for-the-badge&logo=php&logoColor=white)

**A beginner-friendly, hands-on guide to standing up your own web server from scratch**

</div>

---

## 📌 Why This Matters

As a security student, you'll spend a lot of time analyzing servers — but analysis is far more intuitive once you've *built* one yourself. Standing up your own LAMP server teaches you:

- How a real website actually serves pages to a browser
- Where files, permissions, and services live on a Linux box
- What a target server looks like "from the inside" — invaluable when you later study web app security, misconfiguration hunting, or defensive hardening

> 💡 **Learning tip:** Don't just copy-paste the commands. Read the explanation table under each block — that's where the actual understanding happens.

---

## 📖 Table of Contents

<details>
<summary><b>Click to expand</b></summary>

1. [Prerequisites](#-step-1-prerequisites)
2. [Installing the LAMP Stack](#-step-2-installing-the-lamp-stack)
3. [Uploading Your Website Files](#-step-3-uploading-your-website-files)
4. [Fixing File Permissions](#-step-4-fixing-file-permissions)
5. [Configuring the Firewall](#-step-5-configuring-the-firewall)
6. [Testing Your Website](#-step-6-testing-your-website)
7. [Command Reference Cheat Sheet](#-command-reference-cheat-sheet)
8. [Security Notes](#-security-notes)

</details>

---

## 🧩 Step 1: Prerequisites

You'll need one of the following, either as a VM, cloud instance, or spare machine:

| Requirement | Notes |
|---|---|
| Ubuntu Server (recommended) | Latest LTS release works best |
| or Debian | Fully compatible with the same commands |
| `sudo` access | Required for every install step below |

---

## ⚙️ Step 2: Installing the LAMP Stack

**LAMP** = **L**inux + **A**pache + **M**ySQL + **P**HP — the most common stack for serving dynamic websites.

<details open>
<summary><b>🔹 2.1 — Install Apache (the web server)</b></summary>

```bash
sudo apt update
sudo apt install apache2
```

| Command | What it does |
|---|---|
| `sudo` | Runs the command with administrator (root) privileges |
| `apt` | The main package manager tool |
| `update` | Refreshes the list of available package versions |
| `install` | Installs the named software package |
| `apache2` | The software that turns your machine into a web server |

</details>

<details>
<summary><b>🔹 2.2 — Install MySQL (the database server)</b></summary>

```bash
sudo apt install mysql-server
```

| Command | What it does |
|---|---|
| `sudo` | Runs the command with administrator (root) privileges |
| `apt install` | Installs the named software package |
| `mysql-server` | Popular software for creating and managing databases |

</details>

<details>
<summary><b>🔹 2.3 — Install PHP</b></summary>

```bash
sudo apt install php libapache2-mod-php php-mysql
```

| Command | What it does |
|---|---|
| `php` | The programming language used to build dynamic websites |
| `libapache2-mod-php` | Connects PHP to the Apache web server |
| `php-mysql` | Lets PHP communicate with the MySQL database |

</details>

---

## 📂 Step 3: Uploading Your Website Files

All website files live in `/var/www/html/`. Let's create a simple `index.html`:

```bash
sudo nano /var/www/html/index.html
```

| Command | What it does |
|---|---|
| `nano` | A simple terminal text editor |
| `/var/www/html/` | The default folder where Apache looks for website files |
| `index.html` | The homepage file a browser loads by default |

**Example content to paste in:**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>My First Server</title>
</head>
<body>
    <h1>🎉 It works! My LAMP server is live.</h1>
    <p>Hosted from scratch by following the guide.</p>
</body>
</html>
```

Save and exit with `Ctrl + X`, then `Y`, then `Enter`.

---

## 🔐 Step 4: Fixing File Permissions

Apache runs as the `www-data` user — it needs the right ownership and permissions to actually read your files.

```bash
sudo chown -R www-data:www-data /var/www/html/
sudo chmod -R 755 /var/www/html/
```

| Command | What it does |
|---|---|
| `chown` | Changes who *owns* a file or folder |
| `-R` | Applies the command recursively (to every file inside) |
| `www-data:www-data` | Sets Apache's own user & group as the owner |
| `chmod` | Changes read/write/execute *permissions* |
| `755` | Owner: full control · Everyone else: read + execute only |

---

## 🧱 Step 5: Configuring the Firewall

If UFW (Uncomplicated Firewall) is active, open the web ports:

```bash
sudo ufw allow 'Apache Full'
```

| Command | What it does |
|---|---|
| `ufw` | Linux's built-in firewall manager |
| `allow` | Permits a specific type of traffic |
| `'Apache Full'` | A preset that opens both HTTP (port 80) and HTTPS (port 443) |

---

## 🌐 Step 6: Testing Your Website

Find your server's IP address and check it works:

```bash
ip a | grep inet
curl -I http://localhost
```

Then open a browser and visit:

```
http://<your-server-ip>
```

You should see your **"It works!"** page. 🎉

---

## 🗂️ Command Reference Cheat Sheet

| Task | Command |
|---|---|
| Update package lists | `sudo apt update` |
| Install Apache | `sudo apt install apache2` |
| Install MySQL | `sudo apt install mysql-server` |
| Install PHP | `sudo apt install php libapache2-mod-php php-mysql` |
| Edit homepage | `sudo nano /var/www/html/index.html` |
| Fix ownership | `sudo chown -R www-data:www-data /var/www/html/` |
| Fix permissions | `sudo chmod -R 755 /var/www/html/` |
| Open firewall ports | `sudo ufw allow 'Apache Full'` |
| Check Apache status | `sudo systemctl status apache2` |
| Restart Apache | `sudo systemctl restart apache2` |

---

## 🛡️ Security Notes

Hosting your own server is also a great way to *think defensively*:

- Never leave `/var/www/html/` world-writable — that's how unauthorized file uploads happen
- Keep uploaded file-handling logic (if you add any later) strict about file type and extension — loose upload validation is a classic path to remote code execution
- Always run `sudo apt update && sudo apt upgrade` regularly to patch known vulnerabilities
- Disable directory listing in Apache (`Options -Indexes`) unless you need it

---

<div align="center">

**Built as part of my Linux & cybersecurity self-study journey**
*Author: Atia Sanjida*

</div>
