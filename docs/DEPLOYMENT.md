# Detailed Infrastructure Deployment & Setup Guide

This document contains step-by-step instructions for provisioning and properly decommissioning the AWS VPC architecture[cite: 1].

---

## 1. VPC & Subnet Setup

1. **Provision Custom VPC:** Create a VPC named `MyCustomVPC` with IPv4 CIDR `10.0.0.0/16`[cite: 1].
2. **Create Public Subnet:** Create a Subnet named `PublicSubnet-1` with CIDR `10.0.1.0/24` in Availability Zone `us-east-1a`[cite: 1].
3. **Enable Auto-Assign Public IP:** Configure `PublicSubnet-1` settings to automatically assign public IPv4 addresses to launched instances[cite: 1].

![VPC Setup](docs/images/vpc-setup.png)
> 🔴 **Guide Annotations:** Red circle around the **IPv4 CIDR block** (`10.0.0.0/16`) input field and arrow pointing to **Create VPC**[cite: 1].

![Subnet Settings](docs/images/subnet-auto-ip.png)
> 🔴 **Guide Annotations:** Red circle around **Enable auto-assign public IPv4 address** checkbox and arrow pointing to **Save**[cite: 1].

---

## 2. Internet Gateway & Routing

1. **Create & Attach Internet Gateway:** Create an Internet Gateway named `MyIGW` and attach it to `MyCustomVPC`[cite: 1].
2. **Create Custom Route Table:** Create a Route Table named `PublicRouteTable` associated with `MyCustomVPC`[cite: 1].
3. **Configure Default Route:** Add a route targeting `0.0.0.0/0` with `MyIGW` as the target device[cite: 1].
4. **Associate Subnet:** Associate `PublicRouteTable` with `PublicSubnet-1`[cite: 1].

![Attach IGW](docs/images/attach-igw.png)
> 🔴 **Guide Annotations:** Red circle selecting `MyCustomVPC` from the VPC dropdown menu and arrow pointing to **Attach internet gateway**[cite: 1].

![Route Configuration](docs/images/edit-routes.png)
> 🔴 **Guide Annotations:** Red box highlighting `0.0.0.0/0` destination and arrow pointing to the selected `MyIGW` target[cite: 1].

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

---

## 4. EC2 Deployment & SSH Key Troubleshooting

1. **Launch EC2 Instance:** Provision an instance named `MyWebServer` using **Amazon Linux 2023** on a `t2.micro` instance type[cite: 1].
2. **Assign Network Interface:** Bind the instance to `MyCustomVPC`, place it in `PublicSubnet-1`, and attach `WebServerSG`[cite: 1].
3. **Generate SSH Key Pair:** Create and download `my-web-key.pem`[cite: 1].
4. **Convert Key Format (PuTTYgen Fix):** Open PuTTYgen, load `my-web-key.pem`, and export as a 64-bit `my-web-key.ppk` to resolve download key format corruption[cite: 1].
5. **Establish SSH Connection:** Run `chmod 400 my-web-key.pem` and connect using `ssh -i "my-web-key.pem" ec2-user@<YOUR_EC2_PUBLIC_IP>`[cite: 1].

---

## 5. Teardown Protocol (Cost Control)

1. **Terminate EC2 Instance:** Navigate to **EC2 Dashboard** > **Instances**, select `MyWebServer`, click **Instance State** > **Terminate instance**, and wait for status to change to **Terminated**[cite: 1].
2. **Delete Custom VPC:** Navigate to **VPC Dashboard**, select `MyCustomVPC`, click **Actions** > **Delete VPC**, and type `delete` to confirm[cite: 1].
3. **Verify Automated Removal:** Confirm AWS automatically removes associated Subnets, Internet Gateways, Route Tables, and NACLs[cite: 1].
