# Secure AWS Public Web Server Architecture using VPC, NACL & Security Groups
![architecture](diagram/ARCITECHTURE.png)




### VPC Details
![architecture](diagram/vpc.png)

### 🟩 VPC Creation (10.0.0.0/16)

1. Open AWS Console → Search **VPC**.
2. Click **Create VPC**.
3. Choose **VPC Only** option.
4. Enter:
   - Name: My-VPC
   - IPv4 CIDR: 10.0.0.0/16
   - Tenancy: Default
5. Click **Create VPC**.
6. Verify VPC is created with correct CIDR.


### Subnet Details
![subnet](diagram/subnet.png)

### Route Table
![route-table](diagram/Route%20Table.PNG)

### Internet Gateway
![igw](diagram/Net-IGW.png)

### EC2 Instance
![ec2](diagram/ec2.PNG)

### NACL Rules
![nacl](diagram/NACL%20INBOUND%20RULES.PNG)

### Nginx Installed ADMIN 
![nginx](diagram/nginx%20installed.PNG)
### Client IP Details
![client-ip](diagram/CLAINT%20IP.jpeg)

### Client Access HTTP 80 in NGINX
![client-access](diagram/CLAINT%20ACCESS%20HTTP%2080%20IN%20NGINX.jpeg)
