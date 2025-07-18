# Standard Operating Procedure (SOP)
## Setup a virtual Linux Server for Web Apllication Testing

### PURPOSE OF THE DOCUMENT
This SOP outlines the standard procedures for setting up a virtual Linux server environment tailored for web application testing. It ensures consistency in deployment, appropriate configuration, and alignment with testing requirements.

### AUDIENCE
This document is intended for system administrators, QA engineers, DevOps teams, and IT personnel involved in provisioning virtual environments for test automation or manual web application testing.

### APPROVAL TABLE

| Version | Date       | Name                        | Designation |
|---------|------------|-----------------------------|-------------|
| 1.0     | 2025-07-18 | Emmanuel Opoku              | Author      |
| 1.1     | 2025-07-20 | Abiba Sakor                 | Reviewer    |
| 1.2     | 2025-07-21 | Kwesi Poku                  | Approver    |

### SCOPE / OBJECTIVES

This SOP covers the provisioning and configuration of a Linux-based virtual machine used specifically for testing web applications. It includes:
1. Choosing the OS and system requirements
2. Installation of required software packages
3. Configuration of networking and hostname
4. Web server and database setup
5. Deployment-ready testing environment validation

### ACCOUNTABILITY MATRIX

| Task/Steps                          | Responsible Team         | Emergency Contact       | Non-Emergency Contact  | Reviewer          |
|-------------------------------------|---------------------------|--------------------------|------------------------|-------------------|
| VM Setup and OS Installation        | System Administrator      | opokus.rec@gmail.com     | it-support109@gmail.com | IT Manager        |
| Software Stack Installation         | DevOps Team               | abiba.rec@gmail.com.com  | team89@gmail.com        | DevOps Lead       |
| Networking and Security Setup       | Network Engineering Team  | kwesi.rec@gmail.com      | netsec120@gmail.com     | Network Manager   |
| Final Testing and Validation        | QA Team                   | emma.rec@gmail.com       | support49@gmail.com     | QA Manager        |
| Documentation and Handoff          | Documentation Team        | richard.rec@gmail.com     | team89@gmail.com        | Tech Writer Lead  |

### EXECUTION STEPS
## Step 1: Pre-Setup Planning
### 1.1 Define Use Case and Objectives  
- Environment: Internal Web Application Functional Testing  
- Test Goals: Functionality, response time, security, compatibility

### 1.2 Define Resources and VM Specs  
- OS: Ubuntu Server 22.04 LTS  
- Hostname: `web-tester-serverA`  
- CPU: 2 vCPUs  
- RAM: 8 GB  
- Storage: 30 GB  
- Network: NAT (Static IP: `192.168.56.50`)  
- Users: `tester`, `admin`

## Step 2: Provision Virtual Machine

### 2.1 Create VM on VMware  
- Create a new VM named `web-test-serverA`  
- Attach Ubuntu 22.04 ISO  
- Assign resources (CPU, RAM, Disk) as per specs

### 2.2 Install Ubuntu Server  
- Boot from ISO  
- Configure language, keyboard layout  
- Set static IP: `192.168.56.50`, gateway: `192.160.60.2`, and DNS : `192.168.60.2` 
- Create users: `admin` (sudo) and `tester` (non-privileged)  
- Finish installation and reboot

## Step 3: System Update and Software Installation

### 3.1 System Update  
`sudo apt update && sudo apt upgrade -y`

### 3.2 Install Web Server and Testing Tools
```
sudo apt install apache2 -y
sudo apt install php libapache2-mod-php php-mysql -y
sudo apt install mysql-server -y
sudo apt install git curl -y
sudo apt install nodejs npm -y
```
###  Install Docker for containerized the web
``` 
sudo apt install docker.io -y
sudo systemctl enable docker
sudo usermod -aG docker tester
``` 
## Step 4: Network and Security Configuration

### 4.1 Configure UFW Firewall
```
sudo ufw allow OpenSSH
sudo ufw allow Apache Full
sudo ufw enable
```
### 4.2 Enable SSH Access
```
sudo systemctl enable ssh
sudo systemctl start ssh
```
# Step 5: Application Deployment Setup

### 5.1 Setup Sample Web App (PHP)
```
cd /var/www/html
sudo git clone https://github.com/example/web-app-test.git
sudo chown -R www-data:www-data web-app-test
```
### 5.2 Configure Apache Site

#### Create /etc/apache2/sites-available/web-app.conf: 
<VirtualHost *:80>
    ServerName web-test-server.local
    DocumentRoot /var/www/html/web-app-test
    <Directory /var/www/html/web-app-test>
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>

```
sudo a2ensite web-app.conf
sudo a2enmod rewrite
sudo systemctl reload apache2
```
## Step 6: Final Testing & Validation

### 6.1 Functional Test

1. Access site via http://192.168.56.50/
2. Validate PHP info page: phpinfo()
3. Test database connection with sample data

### 6.2 Security Test
1. Check firewall using `sudo ufw status`
2. Verify SSH login, Apache default page removal

## Step 7: Documentation and Handoff
#### 7.1 Document Configuration
Record the following in the internal wiki or documentation tool:
1. VM specs
2. Installed software and versions
3. Network configuration
4. Default credentials and access URLs

7.2 Knowledge Transfer
Share documentation and access credentials with QA and Dev teams.

## REVISION HISTORY
| Version	| Date	| Changed By |	Summary |
|---------|-------|------------|----------|
|1.0   |2025-07-18 |Emmanuel Opoku	| Initial SOP creation |
|1.1	|2025-07-20	|Jane Doe	        |Review and edits |
|1.2	|2025-07-21	|John Smith	      |Final approval and publishing |



