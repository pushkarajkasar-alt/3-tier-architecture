# AWS 3-Tier Architecture Web Application

A fully functional **3-Tier Web Registration using AWS (VPC and EC2-Instances) ** deployed on **Amazon Web Services (AWS)** using a **LEMP Stack (Linux, Nginx, MySQL, PHP)**.  
This project demonstrates how to design, configure, and deploy a secure and scalable student registration system following industry-standard best practices.

## 1. Architecture Overview

The application follows a classic 3-Tier model:

| Tier | Component | Function | Network Placement |
|------|------------|-----------|-------------------|
| **Web Tier** | Nginx + HTML | Handles frontend requests | Public Subnet |
| **Application Tier** | PHP | Processes logic and handles data flow | Private Subnet |
| **Database Tier** | MySQL | Stores data securely | Private Subnet |

**Flow:**  
User → Web Server → App Server → Database Server
## 2. Tech Stack

- **Cloud Platform:** AWS (VPC, Subnets, Route Table, NAT Gateway, Elastic IP,, Internet Gateway, EC2-Instance.)
- **Frontend:** HTML, CSS
- **Backend:** PHP
- **Database:** MySQL
- **Web Server:** Nginx
- **Operating System:** Amazon Linux 2

## 3. AWS Infrastructure Setup

I. **VPC and Subnets**
   - Created a Custom VPC with CIDR `10.0.0.0/16`
   - Created three subnets:
     - `web-subnet` (Public)
     - `app-subnet` (Private)
     - `db-subnet` (Private)
   - Placed each subnet in a different Availability Zone.

II. **Route Tables**
   - Public Route Table → connected to Internet Gateway.
   - Private Route Table → connected to NAT Gateway.

III. **EC2 Instances**
   - **Web Server (Public):** Serves HTML and routes traffic to App Server.
   - **App Server (Private):** Runs PHP for backend processing.
   - **DB Server (Private):** Hosts MySQL database for data storage.
   
## 4. Security Group Configuration

| Server | Allowed Source | Allowed Ports | Purpose |
|---------|----------------|----------------|----------|
| Web Server | 0.0.0.0/0 | 80 (HTTP) | Public access |
| App Server | Web SG | 80 (HTTP) | Accepts traffic from Web Tier |
| DB Server | App SG | 3306 (MySQL) | Accepts traffic from App Tier |

## 5. Deployment Steps

I. **Web Server**
   - Install Nginx:
     ```bash
     sudo yum install nginx -y
     sudo systemctl enable nginx
     sudo systemctl start nginx
     ```
   - Upload `form.html` to `/usr/share/nginx/html/`
   - Configure reverse proxy in `/etc/nginx/nginx.conf`:
     
![php changes config](https://github.com/user-attachments/assets/a57949b5-851e-4f49-8679-94625e4108cb)

   - Restart Nginx:
     ```bash
     sudo systemctl restart nginx
     sudo systemctl reload nginx
     ```

II. **App Server**
   - Install PHP and dependencies:
     ```bash
     sudo yum install php php-mysqlnd mariadb105-server -y
     sudo systemctl enable php-fpm
     sudo systemctl start php-fpm
     ```
   - Upload `submit.php` to `/usr/share/nginx/html/`.

III. **Database Server**
   - Install MySQL:
     ```bash
     sudo yum install mariadb105-server -y
     sudo systemctl enable mariadb
     sudo systemctl start mariadb
     ```
   - Create database and user:
     ```sql
     CREATE DATABASE studentdb;
     CREATE USER 'rushi'@'<APP-SERVER-PRIVATE-IP>' IDENTIFIED BY 'dase';
     GRANT ALL PRIVILEGES ON studentdb.* TO 'rushi'@'<APP-SERVER-PRIVATE-IP>';
     FLUSH PRIVILEGES;

## NOTE - "DELETE THE NAT GATEWAY PLEASE AND RELESE THE ELSTIC IP"

## 6. Verification

- Open in browser: `http://<WEB-SERVER-PUBLIC-IP>/form.html`
- Fill the registration form.
- Check the database entries:
  ```sql
  SELECT * FROM students;

**Note - If data appears correctly, It means deployment is successful.**

### **7. Future Enhancements**
```md
## Future Enhancements
- Add Load Balancer (ELB) for high availability.
- Integrate SSL/TLS using AWS Certificate Manager.
- Automate deployment using Terraform or CloudFormation.
- Monitor infrastructure via AWS CloudWatch.
(still learning so theere are nwe relese's soon)

```
### **8.  Author**

**Pushkaraj kasar**  
Cloud Computing Enthusiast | AWS Learner  
📧 *pushkarajkasar@gmail.com*



