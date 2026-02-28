🔐 AWS Site-to-Site VPN Project (On-Premises ↔ AWS Cloud)

📌 Project Overview
This project demonstrates how to build a Site-to-Site VPN connection between a simulated On-Premises network and an AWS Cloud VPC using IPsec (StrongSwan).
The goal is to establish secure, encrypted communication between two separate networks — similar to how enterprises connect their data centers to AWS in hybrid cloud architecture.

🚀 Technologies Used
- AWS VPC
- EC2 (Ubuntu)
- Site-to-Site VPN
- StrongSwan
- IPsec
- Linux Networking

🏗️ Architecture Design
🔹 On-Premises Network (Simulated)
- VPC CIDR: 172.16.0.0/16
- Public Subnet
- Ubuntu EC2 instance acting as Customer Gateway
- StrongSwan installed for IPsec VPN

🔹 AWS Cloud Network
- VPC CIDR: 10.0.0.0/16
- Private Subnet only
- Private EC2 instance (no public IP)
- Virtual Private Gateway attached

🔹 Connectivity
- Site-to-Site VPN
- Static routing
- IPsec tunnels (UDP 500 & 4500)

🛠️ Implementation Steps
1️⃣ Create On-Premises VPC
- CIDR: 172.16.0.0/16
- 1 Public Subnet
- No NAT Gateway

2️⃣ Launch On-Prem EC2 Instance
- Ubuntu AMI
- Public IP enabled
- Security Group Rules:
     * SSH (22)
     * ICMP (172.16.0.0/16)
     * UDP 500 (IPsec)
     * UDP 4500 (IPsec)
     
3️⃣ Create AWS Cloud VPC
- CIDR: 10.0.0.0/16
- 1 Private Subnet
- No Public Subnet
- No NAT Gateway

4️⃣ Configure VPN Components
✔ Customer Gateway
   - Public IP of On-Prem EC2
   - BGP ASN: Default

✔ Virtual Private Gateway
   - Amazon default ASN
   - Attached to AWS Cloud VPC

✔ Site-to-Site VPN Connection
   - Routing: Static
   - Static IP Prefix: 172.16.0.0/16
   - Download configuration (Vendor: StrongSwan)

🔧 StrongSwan Configuration (On-Prem EC2)
*Install StrongSwan
>sudo apt update
>sudo apt install strongswan -y

*Enable IP Forwarding
>sudo nano /etc/sysctl.conf
# Uncomment: net.ipv4.ip_forward=1
>sudo sysctl -p

* Configure IPsec
Edit:
>sudo nano /etc/ipsec.conf
- Uncomment: uniqueids = no
- Paste conn Tunnel1 configuration
- Add AWS VPC CIDR (10.0.0.0/16)

>sudo nano /etc/ipsec.secrets
- Paste Pre-Shared Keys (Tunnel 1 & 2)

>sudo nano /etc/ipsec.d/aws-updown.sh
>sudo chmod 744 /etc/ipsec.d/aws-updown.sh
>sudo ipsec restart
>sudo ipsec status

🛣️ Route Table Configuration
In AWS VPC: Update Private Route Table
Add route: Destination: 172.16.0.0/16
           Target: Virtual Private Gateway

🖥️ Launch Private EC2 in AWS VPC
Security Group: SSH → 172.16.0.0/16
                ICMP → 172.16.0.0/16

Verified VPN tunnel status using:
>sudo ipsec status
- Confirmed connectivity using ICMP (ping)
- Ensured private communication between networks

🎯 Key Outcomes : 
✔ Successfully established encrypted IPsec tunnels
✔ Enabled secure hybrid cloud communication
✔ Configured routing between two isolated CIDR blocks
✔ Simulated real enterprise Site-to-Site VPN architecture

📚 Key Concepts Learned
- Customer Gateway vs Virtual Private Gateway
- IPsec (IKE Phase 1 & Phase 2 basics)
- Static Routing in VPN
- Security Group & Route Table design
- Hybrid Cloud Networking

📌 Conclusion
This project simulates a real-world enterprise hybrid cloud setup where on-premises infrastructure securely communicates with AWS private resources using Site-to-Site VPN.
It strengthened my understanding of:
 - AWS networking fundamentals
 - Secure connectivity design
 - VPN troubleshooting
 - Infrastructure-level security




