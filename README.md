# 🎫 Helpdesk Ticketing System — osTicket on Ubuntu VM

> A fully functional IT help desk ticketing system built from scratch using osTicket, deployed on an Ubuntu Virtual Machine. This project demonstrates real-world IT support skills including system setup, terminal commands, ticket management and SLA handling.

---

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [What I Learned](#what-i-learned)
- [Tools and Technologies Used](#tools-and-technologies-used)
- [Part 1 — Setting Up the Virtual Machine](#part-1--setting-up-the-virtual-machine)
- [Part 2 — Installing Ubuntu](#part-2--installing-ubuntu)
- [Part 3 — Installing Required Software via Terminal](#part-3--installing-required-software-via-terminal)
- [Part 4 — Installing osTicket](#part-4--installing-osticket)
- [Part 5 — Admin Panel Configuration](#part-5--admin-panel-configuration)
- [Part 6 — Creating and Resolving Tickets](#part-6--creating-and-resolving-tickets)
- [Ticket Priority Guide](#ticket-priority-guide)
- [Key Takeaways](#key-takeaways)

---

## Project Overview

This project involved building a complete help desk ticketing system from the ground up — no shortcuts, no pre-built environments. Every step was done manually, from creating the virtual machine to installing software in the terminal to handling real-world style tickets.

The goal was to simulate what an IT support technician or help desk professional does on the job every day — logging issues, prioritising them correctly, assigning them to agents, and resolving them with clear professional notes.

---

## What I Learned

- How to create and configure a Virtual Machine using VirtualBox
- How to install and navigate Ubuntu Linux
- How to use the Linux terminal to install and manage software
- How to set up a full web stack (Apache, PHP, MySQL)
- How to deploy a web-based application (osTicket) on a local server
- How to configure an IT ticketing system from the admin side
- How to create, assign, prioritise and resolve help desk tickets
- How to write professional resolution notes
- How to apply SLA (Service Level Agreement) plans to tickets
- How to think critically about ticket priority based on business impact

---

## Tools and Technologies Used

| Tool / Technology | Purpose |
|---|---|
| Oracle VirtualBox | Creating and running the Virtual Machine |
| Ubuntu 24 LTS | Operating system running inside the VM |
| Linux Terminal | Installing and managing all software |
| Apache2 | Web server that runs osTicket |
| PHP | Programming language osTicket is built on |
| MySQL | Database that stores all tickets and users |
| osTicket | The help desk ticketing system |
| Firefox | Accessing the osTicket web interface |

---

## Part 1 — Setting Up the Virtual Machine

### What is a Virtual Machine?
A Virtual Machine (VM) is a computer running inside your computer. It lets you run a completely separate operating system without needing a second physical machine. This is perfect for IT home labs because you can safely practice without affecting your main computer.

### Steps
1. Download and install **Oracle VirtualBox** from [virtualbox.org](https://www.virtualbox.org)
2. Click **New** to create a new virtual machine
3. Set the name (e.g. `Ticketing Project`)
4. Set the type to **Linux** and version to **Ubuntu (64-bit)**
5. Allocate at least **2GB RAM** and **20GB storage**
6. Finish the setup wizard

---

## Part 2 — Installing Ubuntu

### What is Ubuntu?
Ubuntu is a free Linux-based operating system. It is widely used in IT environments, especially for servers, making it a great skill to have.

### Steps
1. Download the **Ubuntu ISO** from [ubuntu.com](https://ubuntu.com/download)
2. In VirtualBox, go to your VM settings and mount the ISO as a virtual disc
3. Start the VM — it will boot from the ISO
4. Follow the Ubuntu installation wizard
5. Set your username and password (remember these — you will need them)
6. Let the installation complete and restart the VM

---

## Part 3 — Installing Required Software via Terminal

### What is the Terminal?
The terminal is a text-based way to control your computer. Instead of clicking buttons you type commands. It is a core skill for any IT professional because it gives you more control and is faster for many tasks.

### Step 1 — Update the System
Always do this before installing anything. It makes sure your system has the latest software.

```bash
sudo apt-get update && sudo apt-get upgrade
```

### Step 2 — Install Apache (Web Server)
Apache is what allows osTicket to be accessed through a browser. It acts as the engine powering the web interface.

```bash
sudo apt-get install apache2
```

To check Apache is running, open a browser and go to:
```
http://localhost
```
You should see the Apache default page.

### Step 3 — Install PHP
osTicket is written in PHP. Without it the system cannot run. Each package below adds a specific feature that osTicket requires.

```bash
sudo apt-get install php libapache2-mod-php php-mysql php-xml php-mbstring php-intl php-apcu php-imap php-gd php-curl
```

### Step 4 — Install MySQL (Database)
MySQL is the database that stores all tickets, users, and replies.

```bash
sudo apt-get install mysql-server
```

Secure the MySQL installation:

```bash
sudo mysql_secure_installation
```

### Step 5 — Create a MySQL Database for osTicket

```bash
sudo mysql -u root -p
```

Then inside MySQL run:

```sql
CREATE DATABASE osticket;
CREATE USER 'osticketuser'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON osticket.* TO 'osticketuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

---

## Part 4 — Installing osTicket

1. Download the latest osTicket release from [osticket.com/download](https://osticket.com/download)
2. Extract the files and move them to the web server directory:

```bash
sudo mv upload /var/www/html/osticket
```

3. Set the correct file permissions:

```bash
sudo chmod -R 755 /var/www/html/osticket
sudo chown -R www-data:www-data /var/www/html/osticket
```

4. Copy the sample config file:

```bash
sudo cp /var/www/html/osticket/include/ost-sampleconfig.php /var/www/html/osticket/include/ost-config.php
sudo chmod 0666 /var/www/html/osticket/include/ost-config.php
```

5. Open a browser and go to:
```
http://localhost/osticket
```

6. Follow the setup wizard — enter your database details when prompted

7. After setup is complete, remove the setup folder for security:

```bash
sudo rm -rf /var/www/html/osticket/setup
```

8. Lock down the config file:

```bash
sudo chmod 0644 /var/www/html/osticket/include/ost-config.php
```

---

## Part 5 — Admin Panel Configuration

Log in to the staff panel at:
```
http://localhost/osticket/scp
```

### Departments Created
Departments make sure tickets go to the right team automatically.

| Department | Purpose |
|---|---|
| Support | Technical issues and IT problems |
| Maintenance | Physical and facilities issues |
| Sales | Sales related enquiries |

### Help Topics Configured
Help Topics are the categories users choose when submitting a ticket.

- Report a Problem
- Report a Problem / Access Issue
- General Inquiry
- Feedback

### SLA Plan
An SLA (Service Level Agreement) is a deadline for resolving tickets. It keeps staff accountable.

| SLA Plan | Response Time | Status |
|---|---|---|
| Default SLA | 18 hours | Active |

> **Note:** In a production environment, an Emergency SLA of 1-2 hours would be created for critical issues affecting the whole business.

### Agents
Agents are the staff members who receive and resolve tickets.

- **Jean-luc Batangana** — Support Department

---

## Part 6 — Creating and Resolving Tickets

Three real-world style tickets were created and resolved to demonstrate the full help desk workflow.

---

### 🎫 Ticket 1 — Hardware Failure

| Field | Details |
|---|---|
| Ticket Number | #328123 |
| User | Test User — testuser@email.com |
| Source | Phone |
| Help Topic | Report a Problem |
| Department | Support |
| Priority | Normal |
| SLA Plan | Default SLA (18 hours) |
| Assigned To | Jean-luc Batangana |

**Issue Description:**
> User called in to report that their computer will not turn on. No lights or sounds when pressing the power button.

**Resolution:**
> Visited the user's desk. The power cable was found unplugged from the wall. Plugged the cable back in and the computer turned on successfully. Issue resolved.

**Why Normal priority?**
The issue affects one person but they may still be able to use other devices or work around it temporarily. Not a business-wide emergency.

---

### 🎫 Ticket 2 — Password Reset

| Field | Details |
|---|---|
| Ticket Number | #424911 |
| User | John Smith — john.smith@email.com |
| Source | Phone |
| Help Topic | General Inquiry |
| Department | Support |
| Priority | High |
| SLA Plan | Default SLA (18 hours) |
| Assigned To | Jean-luc Batangana |

**Issue Description:**
> User called in unable to log into their computer. They have forgotten their password and need it reset to regain access.

**Resolution:**
> User's password was reset and a temporary password was sent allowing them to regain access. User was advised to set a new secure password that they can remember.

**Why High priority?**
The user cannot log in at all which means they are completely blocked from doing their job. This is not an emergency because it only affects one person, but it is urgent because they cannot work at all.

---

### 🎫 Ticket 3 — Network Outage (Emergency)

| Field | Details |
|---|---|
| Ticket Number | #543323 |
| User | Office Manager — manager@email.com |
| Source | Phone |
| Help Topic | Report a Problem / Access Issue |
| Department | Support |
| Priority | **Emergency** |
| SLA Plan | Default SLA (18 hours) |
| Assigned To | Jean-luc Batangana |

**Issue Description:**
> Multiple users from the sales department have called in. The entire sales department's network is down and all computers cannot connect to the internet.

**Resolution:**
> Upon inspection, network cables were found unplugged at the router. Cables were plugged back in, all users' computers were tested and confirmed connected to the internet. Emergency resolved at 23:00 on 23/04/26.

**Why Emergency priority?**
This is not just one person — the entire sales department is affected. When a whole team or department cannot work, that costs the business money every minute. This needs to be fixed immediately.

---

## Ticket Priority Guide

Understanding how to prioritise tickets correctly is one of the most important help desk skills.

| Priority | When to use it | Example |
|---|---|---|
| Low | Minor issue, not affecting work | Printer low on ink |
| Normal | Affects one person but they can work around it | Computer running slowly |
| High | One person completely unable to work | Cannot log in to computer |
| Emergency | Whole team or system is down | Entire network outage |

> **The key question to ask:** *How many people does this affect, and how badly does it stop them from working?*

---

## Key Takeaways

- Building a home lab is one of the best ways to develop and prove practical IT skills
- The Linux terminal is a powerful tool — getting comfortable with it opens many doors in IT
- Every ticket tells a story — good documentation helps the whole team, not just the person who fixed it
- Priority is not about how frustrated the user sounds — it is about business impact
- SLA plans keep teams accountable and ensure no ticket gets forgotten
- Resolution notes are just as important as fixing the problem — they create a knowledge base for future issues

---

## Author

**Jean-luc Batangana**  
IT Home Lab Project | April 2026  
Building practical IT skills one project at a time.

---

*This is part of an ongoing IT home lab series. More projects coming soon.*
