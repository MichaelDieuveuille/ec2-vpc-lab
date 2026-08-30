# Detailed Infrastructure Deployment & Setup Guide

This document contains the step-by-step instructions for provisioning the AWS VPC architecture.

## 1. VPC & Subnet Setup
1. Create VPC named `MyCustomVPC` with IPv4 CIDR `10.0.0.0/16`.
2. Create Subnet named `PublicSubnet-1` with CIDR `10.0.1.0/24` in Availability Zone `us-east-1a`.
3. Enable **Auto-assign public IPv4 address** on `PublicSubnet-1`[cite: 1].

## 2. Internet Gateway & Routing
1. Create Internet Gateway named `MyIGW` and attach it to `MyCustomVPC`[cite: 1].
2. Create Route Table named `PublicRouteTable`[cite: 1].
3. Add route: Destination `0.0.0.0/0` -> Target `MyIGW`[cite: 1].
4. Associate `PublicRouteTable` with `PublicSubnet-1`[cite: 1].

## 3. Dual-Layer Firewall Configuration
1. **Network ACL (`MyCustomNACL`):**
   * Inbound Rule 100: Allow HTTP (80) from `0.0.0.0/0`[cite: 1]
   * Inbound Rule 110: Allow SSH (22) from your IP[cite: 1]
   * Associate with `PublicSubnet-1`[cite: 1]
2. **Security Group (`WebServerSG`):**
   * Inbound Rule 1: Allow HTTP (80) from `0.0.0.0/0`[cite: 1]
   * Inbound Rule 2: Allow SSH (22) from `My IP`[cite: 1]

## 4. EC2 Deployment & SSH Access
1. Launch EC2 Instance `MyWebServer` using **Amazon Linux 2023** on `t2.micro`[cite: 1].
2. Select `MyCustomVPC`, `PublicSubnet-1`, and attach `WebServerSG`[cite: 1].
3. Generate key pair `my-web-key.pem`[cite: 1].
4. **Key Conversion (PuTTYgen):** Open PuTTYgen -> Load `my-web-key.pem` -> Save private key as `my-web-key.ppk`[cite: 1].
5. Connect via SSH:
   ```bash
   chmod 400 my-web-key.pem
   ssh -i "my-web-key.pem" ec2-user@<YOUR_EC2_PUBLIC_IP>
