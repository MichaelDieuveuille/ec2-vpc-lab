# AWS VPC Infrastructure & Public Web Server Deployment

[![AWS](https://img.shields.io/badge/AWS-VPC%20%7C%20EC2%20%7C%20IGW-orange?style=flat-square&logo=amazon-aws)](https://aws.amazon.com/)
[![Networking](https://img.shields.io/badge/Networking-CIDR%20%7C%20Route%20Tables%20%7C%20NACLs-blue?style=flat-square)](https://aws.amazon.com/vpc/)
[![Security](https://img.shields.io/badge/Security-Stateful%20SG%20%7C%20Stateless%20NACL-red?style=flat-square)](https://aws.amazon.com/security/)
[![License](https://img.shields.io/badge/License-MIT-green.svg?style=flat-square)](LICENSE)

A custom AWS Virtual Private Cloud (VPC) architecture built from scratch. This project demonstrates hands-on implementation of cloud-defined networking, dual-layer security perimeter enforcement (stateless NACLs and stateful Security Groups), EC2 instance provisioning, and SSH key troubleshooting.

Designed as a core portfolio asset for entry-level **Cloud Engineer**, **AWS Solutions Architect**, and **Cloud System Administrator** roles.

---

## 🏛️ Architecture Overview

The architecture implements a custom isolated network environment in AWS, complete with public subnets, edge routing, dual-layer firewall enforcement, and an Amazon Linux 2023 web server host.

### On-Premises vs. AWS Cloud Mapping

| Infrastructure Component | AWS Cloud Implementation | On-Premises Data Center Equivalent |
| :--- | :--- | :--- |
| **Network Space** | Custom VPC (`10.0.0.0/16`) | Physical Data Center, Core Switch & LAN IP Scheme |
| **Network Segment** | Public Subnet (`10.0.1.0/24`) | Physical VLAN & Managed Switch Trunk Port |
| **Internet Egress/Ingress** | Internet Gateway (`MyIGW`) | Border Edge Router (Cisco ASR) & ISP Fiber Circuit |
| **Routing** | Route Table (`0.0.0.0/0` -> IGW) | Core Router Static Default Route (`ip route 0.0.0.0 ...`) |
| **Subnet Security** | Stateless Network ACL (`MyCustomNACL`) | Hardware Perimeter Firewall / Router VLAN ACLs |
| **Host Security** | Stateful Security Group (`WebServerSG`) | Host OS Firewall (`iptables`/`ufw`) / Hypervisor vNIC Firewall |
| **Compute** | EC2 (`t2.micro` / Amazon Linux 2023) | 1U/2U Bare-Metal Rack Server or Local VMware/Hyper-V VM |

---

## 📸 Key Implementation & UI Verification Highlights

Below are visual architectural references representing the deployment and troubleshooting phases.

### 1. EC2 Instance Running & Network Binding
*Verification of EC2 deployment inside `MyCustomVPC` with `PublicSubnet-1` and auto-assigned IPv4.*

### 2. Troubleshooting & Resolving SSH Key Authentication Issues
During deployment, standard PuTTY operations against laboratory-provided `.ppk` private key files encountered corruption and invalid format errors (`Unable to use key file: Old or unsupported format`). 

**Root Cause:** Laboratory environment exports for `.ppk` files were corrupted during generator stream output, whereas the raw OpenSSH `.pem` payload retained full cryptographic integrity.

**Engineered Resolution:** Converted the valid `.pem` RSA key pair into a fully functional 64-bit PuTTY SSH key file (`.ppk`) using PuTTYgen.

---

## 📂 Project Documentation

* **Detailed Setup Instructions:** See [docs/DEPLOYMENT.md](file:///C:/Users/Michael/Desktop/Portfolio%20Website%20Guide%20and%20Cloud%20Infras%20Lab.pdf) for full step-by-step deployment instructions.
* **Portfolio Website:** Hosted live on [GitHub Pages](https://Michaeldieuveuille.github.io).

---

## 🙋‍♂️ Author

**Cloud Infrastructure Specialist**
* AWS re/Start Student
* **LinkedIn:** `https://linkedin.com/in/michaeldieuveuille`
* **GitHub:** `https://github.com/MichaelDieuveuille`
