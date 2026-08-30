# AWS VPC Infrastructure & Public Web Server Deployment

[![AWS](https://img.shields.io/badge/AWS-VPC%20%7C%20EC2%20%7C%20IGW-orange?style=flat-square&logo=amazon-aws)](https://aws.amazon.com/)
[![Networking](https://img.shields.io/badge/Networking-CIDR%20%7C%20Route%20Tables%20%7C%20NACLs-blue?style=flat-square)](https://aws.amazon.com/vpc/)
[![Security](https://img.shields.io/badge/Security-Stateful%20SG%20%7C%20Stateless%20NACL-red?style=flat-square)](https://aws.amazon.com/security/)

A custom AWS Virtual Private Cloud (VPC) built from scratch, demonstrating hands-on cloud networking, dual-layer firewall security, and EC2 server deployment. Designed as a portfolio project for entry-level **Cloud Engineer** and **AWS Solutions Architect** roles.

---

## 🏛️ Architecture Overview

The topology deploys a public web server inside a custom isolated network with dedicated edge routing and two-tier firewall protection.

### Infrastructure Summary

| Component | AWS Resource | Details / Settings | On-Premises Equivalent |
| :--- | :--- | :--- | :--- |
| **VPC** | `MyCustomVPC` | CIDR `10.0.0.0/16` (65,536 IPs)[cite: 1] | Data Center Network / Core Switch[cite: 1] |
| **Subnet** | `PublicSubnet-1`[cite: 1] | CIDR `10.0.1.0/24` (Auto-Assign Public IP)[cite: 1] | Physical VLAN Segment[cite: 1] |
| **Gateway** | `MyIGW`[cite: 1] | Attached to VPC for Internet Traffic[cite: 1] | Edge Router / ISP Fiber Link[cite: 1] |
| **Firewall (Subnet)** | `MyCustomNACL`[cite: 1] | Stateless (Inbound HTTP/80 & SSH/22)[cite: 1] | Perimeter Hardware Firewall[cite: 1] |
| **Firewall (Host)** | `WebServerSG`[cite: 1] | Stateful (Inbound HTTP/80 & SSH/22)[cite: 1] | Host OS Firewall (`iptables`)[cite: 1] |
| **Compute** | `MyWebServer`[cite: 1] | EC2 `t2.micro` (Amazon Linux 2023)[cite: 1] | Physical Rack Server / Local VM[cite: 1] |

---

## 🛠️ Key Technical Problem Solved

* **Issue:** The lab-provided PuTTY `.ppk` SSH key was corrupted (`Unable to use key file: Old or unsupported format`)[cite: 1].
* **Root Cause:** Key stream export corruption during download[cite: 1].
* **Solution:** Downloaded the uncorrupted `.pem` OpenSSH key and converted it into a functional 64-bit `.ppk` file using **PuTTYgen**, restoring secure SSH access to the server[cite: 1].

---

## 📂 Project Documentation

* **Detailed Deployment Steps:** See [docs/DEPLOYMENT.md](docs/DEPLOYMENT.md) for full setup instructions[cite: 1].
* **Portfolio Website:** Hosted live on [GitHub Pages](https://YOUR_USERNAME.github.io)[cite: 1].

---

## 🙋‍♂️ Author

**Cloud Infrastructure Specialist**
* AWS re/Start Graduate[cite: 1]
* **LinkedIn:** `https://linkedin.com/in/michaeldieuveuille`[cite: 1]
* **GitHub:** `https://github.com/MichaelDieuveuille`[cite: 1]
