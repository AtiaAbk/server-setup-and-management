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

> 💡 **Learning tip:** Don't just copy-paste the commands. Read the CLI breakdown under each block — that's where the actual understanding happens.

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

- **Ubuntu Server** (recommended) — latest LTS release works best
- or **Debian** — fully compatible with the same commands
- **`sudo` access** — required for every install step below

---

## ⚙️ Step 2: Installing the LAMP Stack

**LAMP** = **L**inux + **A**pache + **M**ySQL + **P**HP — the most common stack for serving dynamic websites.

<details open>
<summary><b>🔹 2.1 — Install Apache (the web server)</b></summary>

```bash
$ sudo apt update
$ sudo apt install apache2
```

```console
sudo            → run with administrator (root) privileges
apt             → the main package manager tool
update          → refresh the list of available package versions
install         → install the named software package
apache2         → software that turns this machine into a web server
```

</details>

<details>
<summary><b>🔹 2.2 — Install MySQL (the database server)</b></summary>

```bash
$ sudo apt install mysql-server
```

```console
sudo            → run with administrator (root) privileges
apt install     → install the named software package
mysql-server    → software for creating and managing databases
```

</details>

<details>
<summary><b>🔹 2.3 — Install PHP</b></summary>

```bash
$ sudo apt install php libapache2-mod-php php-mysql
```

```console
php                     → the programming language used to build dynamic sites
libapache2-mod-php      → connects PHP to the Apache web server
php-mysql               → lets PHP talk to the MySQL database
```

</details>

---

## 📂 Step 3: Uploading Your Website Files

All website files live in `/var/www/html/`. Let's create a simple `index.html`:

```bash
$ sudo nano /var/www/html/index.html
```

```console
nano                → simple terminal text editor
/var/www/html/      → default folder Apache looks in for website files
index.html          → homepage file a browser loads by default
```

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
$ sudo chown -R www-data:www-data /var/www/html/
$ sudo chmod -R 755 /var/www/html/
```

```console
chown                     → change who owns a file or folder
-R                        → apply recursively (to every file inside)
www-data:www-data         → set Apache's own user & group as owner
chmod                     → change read/write/execute permissions
755                       → owner: full control · others: read + execute only
```

---

## 🧱 Step 5: Configuring the Firewall

If UFW (Uncomplicated Firewall) is active, open the web ports:

```bash
$ sudo ufw allow 'Apache Full'
```

```console
ufw                → Linux's built-in firewall manager
allow              → permit a specific type of traffic
'Apache Full'      → preset that opens both HTTP (80) and HTTPS (443)
```

---

## 🌐 Step 6: Testing Your Website

Find your server's IP address and check it works:

```bash
$ ip a | grep inet
$ curl -I http://localhost
```

```console
$ curl -I http://localhost
HTTP/1.1 200 OK
Date: Wed, 23 Sep 2026 10:12:44 GMT
Server: Apache/2.4.58 (Ubuntu)
Content-Type: text/html; charset=UTF-8
```

Then open a browser and visit:

```
http://<your-server-ip>
```

You should see your **"It works!"** page. 🎉

---

## 🗂️ Command Reference Cheat Sheet

```console
$ sudo apt update                                        # refresh package lists
$ sudo apt install apache2                                # install Apache
$ sudo apt install mysql-server                            # install MySQL
$ sudo apt install php libapache2-mod-php php-mysql        # install PHP
$ sudo nano /var/www/html/index.html                        # edit homepage
$ sudo chown -R www-data:www-data /var/www/html/            # fix ownership
$ sudo chmod -R 755 /var/www/html/                           # fix permissions
$ sudo ufw allow 'Apache Full'                                # open firewall ports
$ sudo systemctl status apache2                                # check Apache status
$ sudo systemctl restart apache2                                # restart Apache
```

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
*Author: Atia Abk*

</div>
