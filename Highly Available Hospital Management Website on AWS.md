Since you're practicing Linux and AWS DevOps, here's a **real-world challenge project** that combines Linux administration, networking, permissions, web hosting, S3, Load Balancer, Auto Scaling, and database deployment.

# Project: Highly Available Hospital Management Website on AWS

## Architecture

```text
Internet
    │
    ▼
Application Load Balancer
    │
    ▼
Target Group
    │
 ┌──┴──┐
 ▼     ▼
EC2-1  EC2-2
Apache/Nginx
    │
    ▼
RDS MySQL
    │
    ▼
S3 Bucket
(Images, Backups, Logs)
```

---

# Phase 1: Linux Administration

## Create Users

```bash
sudo useradd devops
sudo passwd devops

sudo useradd developer
sudo passwd developer
```

Check users:

```bash
cat /etc/passwd
id devops
```

---

## Create Directories

```bash
mkdir project
mkdir -p project/{html,css,js,images}

mkdir -p /opt/website
mkdir -p /opt/backups
mkdir -p /opt/scripts
```

View:

```bash
tree
ls -ltr
```

---

## File Operations

```bash
touch index.html
cp index.html backup.html
mv backup.html old.html
rm old.html
```

---

## Permissions

### Numeric Method

```bash
chmod 777 file
chmod 755 directory
chmod 644 index.html
```

### Symbolic Method

```bash
chmod u+x script.sh
chmod g+w file.txt
chmod o-r file.txt
```

### Ownership

```bash
chown ec2-user:ec2-user file.txt
chown -R apache:apache /var/www/html
```

Check:

```bash
ls -l
```

---

# Phase 2: Linux Monitoring

## CPU

```bash
top
htop
mpstat
```

## Memory

```bash
free -m
vmstat
```

## Disk

```bash
df -h
du -sh *
lsblk
```

## Process

```bash
ps -ef
ps aux
kill PID
kill -9 PID
```

---

# Phase 3: Networking Commands

## Network Configuration

```bash
ip a
ip addr
ifconfig
hostname
hostnamectl
```

---

## Routing

```bash
ip route
route -n
```

---

## Connectivity

```bash
ping google.com
```

```bash
traceroute google.com
```

```bash
tracepath google.com
```

---

## DNS

```bash
nslookup google.com
dig google.com
host google.com
```

---

## Ports

```bash
ss -tulpn
netstat -tulpn
lsof -i
```

---

## Connection Testing

```bash
curl http://localhost
curl ifconfig.me

wget https://google.com
```

---

## Firewall

### Amazon Linux

```bash
sudo systemctl status firewalld

sudo firewall-cmd --list-all

sudo firewall-cmd --add-port=80/tcp --permanent

sudo firewall-cmd --reload
```

---

# Phase 4: Web Server Deployment

## Install Apache

Amazon Linux:

```bash
sudo yum update -y
sudo yum install httpd -y

sudo systemctl start httpd
sudo systemctl enable httpd
```

Check:

```bash
systemctl status httpd
```

---

## Create Website

```bash
cd /var/www/html

sudo nano index.html
```

Example:

```html
<h1>Hospital Management System</h1>
```

Test:

```bash
curl localhost
```

---

# Phase 5: GitHub Deployment

Install Git:

```bash
sudo yum install git -y
```

Clone Repository:

```bash
git clone https://github.com/USERNAME/PROJECT.git
```

Move files:

```bash
sudo cp -r PROJECT/* /var/www/html/
```

---

# Phase 6: S3 Operations

## Create Bucket

```bash
aws s3 mb s3://hospital-management-bucket
```

Upload:

```bash
aws s3 cp image.jpg s3://hospital-management-bucket
```

Sync:

```bash
aws s3 sync /var/www/html s3://hospital-management-bucket
```

List:

```bash
aws s3 ls
```

---

# Phase 7: MySQL Database

Install:

```bash
sudo yum install mariadb105-server -y

sudo systemctl start mariadb
sudo systemctl enable mariadb
```

Login:

```bash
mysql -u root -p
```

Create DB:

```sql
CREATE DATABASE hospital_db;

USE hospital_db;

CREATE TABLE patients (
id INT PRIMARY KEY AUTO_INCREMENT,
name VARCHAR(100),
disease VARCHAR(100)
);
```

---

# Phase 8: AWS RDS

Create:

* MySQL RDS
* Public Access = No
* Same VPC as EC2

Connect:

```bash
mysql -h RDS-ENDPOINT \
-u admin \
-p
```

Verify:

```sql
SHOW DATABASES;
```

---

# Phase 9: Load Balancer

### Create 2 EC2 Instances

Install Apache on both.

Instance 1:

```html
<h1>Server 1</h1>
```

Instance 2:

```html
<h1>Server 2</h1>
```

---

### Create Target Group

Health Check:

```text
Path: /
Port: 80
```

Register EC2 instances.

---

### Create ALB

Listener:

```text
HTTP : 80
```

Attach Target Group.

Test:

```bash
curl ALB-DNS-NAME
```

Refresh browser and verify traffic distribution.

---

# Phase 10: Auto Scaling

Create Launch Template.

Configure:

```text
AMI = Amazon Linux
Instance Type = t3.micro
```

User Data:

```bash
#!/bin/bash

yum update -y
yum install httpd -y

systemctl start httpd
systemctl enable httpd

echo "<h1>Auto Scaling Server</h1>" > /var/www/html/index.html
```

Auto Scaling Group:

```text
Min = 2
Desired = 2
Max = 4
```

Attach Target Group.

---

# Phase 11: Automation Script

Create:

```bash
nano healthcheck.sh
```

```bash
#!/bin/bash

echo "Hostname:"
hostname

echo "Date:"
date

echo "Disk Usage:"
df -h

echo "Memory:"
free -m

echo "Top Processes:"
ps aux --sort=-%cpu | head
```

Run:

```bash
chmod +x healthcheck.sh
./healthcheck.sh
```

---

# Final Challenge (Interview-Level)

Build the complete solution:

✅ GitHub Hospital Management Website
✅ 2 EC2 instances
✅ Apache/Nginx
✅ RDS MySQL Database
✅ S3 Bucket for images and backups
✅ Application Load Balancer
✅ Target Group Health Checks
✅ Auto Scaling Group (2–4 instances)
✅ CloudWatch Monitoring
✅ Linux backup scripts
✅ Cron jobs for automated backups
✅ IAM Role for EC2 → S3 access
✅ HTTPS using SSL certificate via AWS Certificate Manager

Completing this end-to-end project will give you hands-on experience with most Linux and AWS concepts commonly tested in DevOps, Cloud Support, and System Administrator interviews.
