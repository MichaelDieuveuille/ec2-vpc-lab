# Detailed Infrastructure Deployment & Setup Guide

This document contains step-by-step instructions for provisioning and properly decommissioning the AWS VPC architecture[cite: 1].

---

## 1. VPC & Subnet Setup

1. **Provision Custom VPC:** Create a VPC named `MyCustomVPC` with IPv4 CIDR `10.0.0.0/16`[cite: 1].
2. **Create Public Subnet:** Create a Subnet named `PublicSubnet-1` with CIDR `10.0.1.0/24` in Availability Zone `us-east-1a`[cite: 1].
3. **Enable Auto-Assign Public IP:** Configure `PublicSubnet-1` settings to automatically assign public IPv4 addresses to launched instances[cite: 1].

VPC Setup : <img width="2136" height="1251" alt="Screenshot 2026-08-30 130919" src="https://github.com/user-attachments/assets/93e637a7-7125-457f-a43c-789a422ddc51" />


Subnet Settings: <img width="2559" height="1268" alt="Screenshot 2026-08-30 131105" src="https://github.com/user-attachments/assets/6cec0c46-d97b-43eb-a31a-dacc8c384008" />


---

## 2. Internet Gateway & Routing

1. **Create & Attach Internet Gateway:** Create an Internet Gateway named `MyIGW` and attach it to `MyCustomVPC`[cite: 1].
2. **Create Custom Route Table:** Create a Route Table named `PublicRouteTable` associated with `MyCustomVPC`[cite: 1].
3. **Configure Default Route:** Add a route targeting `0.0.0.0/0` with `MyIGW` as the target device[cite: 1].
4. **Associate Subnet:** Associate `PublicRouteTable` with `PublicSubnet-1`[cite: 1].

Attach IGW: <img width="2559" height="1296" alt="Screenshot 2026-08-30 131237" src="https://github.com/user-attachments/assets/76e9c270-12b2-49fa-8e0a-c316df6ddf38" />


Route Configuration: <img width="2559" height="1296" alt="Screenshot 2026-08-30 131237" src="https://github.com/user-attachments/assets/48c3b532-99f6-436f-860b-e210d7486b1d" />


---

## 3. Dual-Layer Firewall Configuration & Ephemeral Port Fix

1. **Configure Stateless Network ACL (`MyCustomNACL`):**
   * **Inbound Rule 100:** Allow HTTP (80) from `0.0.0.0/0`[cite: 1].
   * **Inbound Rule 110:** Allow SSH (22) from your IP[cite: 1].
   * **Inbound Rule 120 (Troubleshooting Fix):** Allow Custom TCP (`1024-65535`) from `0.0.0.0/0` to allow returning client traffic (ephemeral ports).
   * **Outbound Rule 100:** Allow Custom TCP (`1024-65535`) to `0.0.0.0/0`.
   * Associate `MyCustomNACL` with `PublicSubnet-1`[cite: 1].
2. **Configure Stateful Security Group (`WebServerSG`):**
   * **Inbound Rule 1:** Allow HTTP (80) from `0.0.0.0/0`[cite: 1].
   * **Inbound Rule 2:** Allow SSH (22) from `My IP`[cite: 1].

Network ACL Configuration: <img width="2557" height="1303" alt="Screenshot 2026-08-30 131637" src="https://github.com/user-attachments/assets/efea40cc-deb4-4a18-84a2-a901ef58feae" />
    
Inbound Rules: <img width="2559" height="1292" alt="Screenshot 2026-08-30 132515" src="https://github.com/user-attachments/assets/48fcc308-396c-4879-9c3a-1c333765a1fe" />

Outbound Rules: <img width="2559" height="1302" alt="Screenshot 2026-08-30 132603" src="https://github.com/user-attachments/assets/033cd681-a238-457a-9de8-91bcde19c844" />


Create Network Security Group: <img width="2559" height="1302" alt="Screenshot 2026-08-30 132603" src="https://github.com/user-attachments/assets/3ac1dfc1-764e-4a7b-b852-affc8a290e5f" />

---

## 4. EC2 Deployment & SSH Key Troubleshooting

1. **Launch EC2 Instance:** Provision an instance named `MyWebServer` using **Amazon Linux 2023** on a `t2.micro` instance type[cite: 1].
2. **Assign Network Interface:** Bind the instance to `MyCustomVPC`, place it in `PublicSubnet-1`, and attach `WebServerSG`[cite: 1].
3. **Generate SSH Key Pair:** Create and download `my-web-key.pem`[cite: 1].
4. **Convert Key Format (PuTTYgen Fix):** Open PuTTYgen, load `my-web-key.pem`, and export as a 64-bit `my-web-key.ppk` to resolve download key format corruption[cite: 1].
5. **Establish SSH Connection:** Run `chmod 400 my-web-key.pem` and connect using `ssh -i "my-web-key.pem" ec2-user@<YOUR_EC2_PUBLIC_IP>`[cite: 1].

Launch EC2 Instance: <img width="2559" height="1303" alt="Screenshot 2026-08-30 132831" src="https://github.com/user-attachments/assets/e8276b6f-e1d3-41b0-abda-c12d5fa05dd4" />


Create a Key Pair: <img width="2559" height="1311" alt="Screenshot 2026-08-30 132922" src="https://github.com/user-attachments/assets/9826e6fa-382a-4255-89e2-2605f2b48edc" />


Network Configuration: <img width="2551" height="1299" alt="Screenshot 2026-08-30 133232" src="https://github.com/user-attachments/assets/634ec9cd-35fa-4618-b9fd-fcb81393631a" />


Putty Configuration: <img width="2551" height="1299" alt="Screenshot 2026-08-30 133232" src="https://github.com/user-attachments/assets/5eab27f4-e039-457d-8d41-6bb6cb1a6168" />


Connection Successful: 


<img width="659" height="416" alt="Screenshot 2026-08-30 133428" src="https://github.com/user-attachments/assets/be602124-b28c-4782-96cc-28921e2ab91f" />

---

## 5. Teardown Protocol (Cost Control)

1. **Terminate EC2 Instance:** Navigate to **EC2 Dashboard** > **Instances**, select `MyWebServer`, click **Instance State** > **Terminate instance**, and wait for status to change to **Terminated**[cite: 1].
2. **Delete Custom VPC:** Navigate to **VPC Dashboard**, select `MyCustomVPC`, click **Actions** > **Delete VPC**, and type `delete` to confirm[cite: 1].
3. **Verify Automated Removal:** Confirm AWS automatically removes associated Subnets, Internet Gateways, Route Tables, and NACLs[cite: 1].

Terminating Instance: <img width="2559" height="1301" alt="Screenshot 2026-08-30 133459" src="https://github.com/user-attachments/assets/4b0036ea-6c99-4e09-ae23-db380c5112b6" />


Deleting VPC: <img width="2559" height="1301" alt="Screenshot 2026-08-30 133533" src="https://github.com/user-attachments/assets/54702942-165b-41ec-8ee3-b41028b7e8a9" />

