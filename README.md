# Secure AWS Public Web Server Architecture using VPC, NACL & Security Groups

## 🧭 Table of Contents
1. [Main Architecture Diagram](#-main-architecture-diagram)
2. [VPC Creation](#-vpc-creation-1000016)
3. [Subnet Creation](#-subnet-creation-1000024)
4. [Internet Gateway Setup](#-internet-gateway-setup-my-igw)
5. [Route Table Setup](#-route-table-setup-public-rt)
6. [Network ACL Setup](#-network-acl-nacl-setup)
7. [Security Group Setup](#-security-group-setup-webserver-sg)
8. [EC2 Instance Launch](#️-ec2-instance-amazon-linux-2)
9. [NGINX Installation](#-install-nginx-on-ec2)
10. [Client Testing](#-client-testing-browser--ssh)

---

## 🖼️ Main Architecture Diagram
![main-architecture](diagram/vpc.png)

---

## 🟩 VPC Creation (10.0.0.0/16)
![vpc](diagram/vpc.png)

1. Open AWS Console → Search **VPC**.
2. Click **Create VPC**.
3. Choose **VPC Only** option.
4. Enter:
   - Name: **My-VPC**
   - IPv4 CIDR: **10.0.0.0/16**
   - Tenancy: Default
5. Click **Create VPC**.
6. Verify VPC is created with correct CIDR.

---

## 🟦 Subnet Creation (10.0.0.0/24)
![subnet](diagram/subnet.png)

1. Go to **VPC → Subnets → Create Subnet**.
2. Select VPC: **My-VPC**.
3. Enter:
   - Name: **Public-Subnet**
   - Availability Zone: ap-south-1a (optional)
   - IPv4 CIDR block: **10.0.0.0/24**
4. Click **Create Subnet**.
5. Select the subnet → **Actions → Edit subnet settings**.
6. Enable **Auto-assign Public IPv4**.

---

## 🌐 Internet Gateway Setup (My-IGW)
![igw](diagram/Net-IGW.png)

1. Go to **VPC → Internet Gateways → Create Internet Gateway**.
2. Enter:
   - Name: **My-IGW**
3. Click **Create IGW**.
4. Select IGW → **Actions → Attach to VPC**.
5. Select **My-VPC**.

---

## 🛣️ Route Table Setup (Public-RT)
![route-table](diagram/Route%20Table.PNG)

1. Go to **VPC → Route Tables → Create Route Table**.
2. Enter:
   - Name: **Public-RT**
   - VPC: **My-VPC**
3. Click **Create Route Table**.

### Add Internet Route
4. Open Public-RT → Go to **Routes**.
5. Click **Edit Routes → Add Route**.
   - Destination: **0.0.0.0/0**
   - Target: **Internet Gateway (My-IGW)**
6. Save the route.

### Associate Subnet
7. Go to **Subnet Associations → Edit**.
8. Select **Public-Subnet**.
9. Save changes.

---

## 🚧 Network ACL (NACL) Setup
![nacl](diagram/NACL%20INBOUND%20RULES.PNG)

1. Go to **VPC → Network ACLs → Create NACL**.
2. Name: **Public-NACL**
3. Select VPC: **My-VPC**

### Associate Subnet
4. Open NACL → **Subnet Associations → Edit**.
5. Select **Public-Subnet** → Save.

### Inbound Rules
| Rule | Port | Source | Allow |
|------|------|---------|--------|
| 100 | 80 | 0.0.0.0/0 | ALLOW |
| 110 | 22 | Your IP Only | ALLOW |
| 120 | 1024-65535 | 0.0.0.0/0 | ALLOW |

### Outbound Rules
| Rule | Port | Destination | Allow |
|------|------|-------------|--------|
| 100 | ALL | 0.0.0.0/0 | ALLOW |

---




1. Go to **EC2 → Security Groups → Create Security Group**.
2. Enter:
   - Name: **WebServer-SG**
   - VPC: **My-VPC**

### Inbound Rules
| Type | Port | Source |
|------|------|---------|
| HTTP | 80 | 0.0.0.0/0 |
| SSH  | 22 | Your IP Only |

### Outbound
| Type | Port | Destination |
|------|------|-------------|
| ALL | ALL | 0.0.0.0/0 |

---

## 🖥️ EC2 Instance (Amazon Linux 2)
![ec2](diagram/ec2.PNG)

1. Go to **EC2 → Launch Instance**.
2. Name: **Web-Server**
3. AMI: **Amazon Linux 2**
4. Instance Type: **t2.micro**
5. Select:
   - VPC: **My-VPC**
   - Subnet: **Public-Subnet**
   - Auto-assign Public IP: YES
6. Security Group: **WebServer-SG**
7. Launch with key pair.

---

## 🚀 Install NGINX on EC2
![nginx](diagram/nginx%20installed.PNG)

Run the following commands:

```bash
sudo yum update -y
sudo amazon-linux-extras enable nginx1
sudo yum install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
```md
---

## 🧪 Client Testing (Browser & SSH)

### 🔹 Step 1: Copy EC2 Public IP
1. Go to **EC2 → Instances**.
2. Select your instance.
3. Copy the **Public IPv4 address**.

---

### 🔹 Step 2: Test HTTP (Port 80) in Browser
1. Open Chrome / Edge / Firefox.
2. Enter your EC2 public IP:

3. Expected Result:
- **NGINX Default Welcome Page** should load.
- This confirms:
  - IGW → Working  
  - Route Table → Working  
  - NACL inbound 80 → Allowed  
  - SG inbound 80 → Allowed  
  - EC2 → Running OK  

---

If SSH works → Security Group + NACL SSH rules correct.

---

### 🔹 Screenshot: Client HTTP Test  
![client-access](diagram/CLAINT%20ACCESS%2080%20IN%20NGINX.jpeg)

### 🔹 Screenshot: Client IP  
![client-ip](diagram/CLAINT%20IP.jpeg)


