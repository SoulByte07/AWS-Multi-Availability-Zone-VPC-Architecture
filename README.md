# AWS Multi-Availability Zone VPC Architecture

Building a resilient, "self-healing" infrastructure starts at the networking layer. This project demonstrates a **3-Tier VPC Architecture** deployed across multiple Availability Zones (AZs). By mirroring resources, we ensure that even if an entire AWS data center goes offline, the application remains highly available and secure.

---

## ## Architecture Overview

The design follows the "Gold Standard" for cloud networking: a 3-tier segmentation that separates the entry point, the logic, and the data.

### ### Key Features

- **High Availability:** Resources are redundant across two geographic locations (e.g., `us-east-1a` and `us-east-1b`).
    
- **Security Isolation:** Databases are tucked away in a private "Data Tier" with no direct internet access.
    
- **Controlled Outbound Traffic:** Private instances can download updates via a NAT Gateway without being exposed to the public web.
    

---

## ## Component Differentiator

|**Category**|**Component**|**Purpose**|**Access Level**|
|---|---|---|---|
|**Public Tier**|Web Subnets|Houses Load Balancers & Bastion Hosts.|Direct Internet Access|
|**Private Tier**|App Subnets|Where the application logic (EC2) lives.|Outbound via NAT Only|
|**Data Tier**|DB Subnets|Strictly for Databases (RDS).|Internal VPC Access Only|
|**Connectivity**|IGW & NAT|The "Front Door" and "One-Way Exit" for traffic.|Infrastructure Layer|

---

## ## Implementation Guide (Manual Console)

This architecture was built using the AWS Management Console to ensure a deep understanding of the underlying networking handshakes.

1. **VPC Setup:** Established a `10.0.0.0/16` CIDR block (offering 65,536 IPs).
    
2. **Subnetting:** Created 6 subnets distributed across two AZs for maximum redundancy.
    
3. **Gateways:** Attached an **Internet Gateway (IGW)** for the public tier and deployed a **NAT Gateway** in a public subnet for the private tiers.
    
4. **Routing:**
    
    - **Public Route Table:** Pointed to the IGW.
        
    - **Private Route Table:** Pointed to the NAT Gateway.
        
5. **Security:** Implemented Security Groups to allow only specific traffic (e.g., Port 80/443 for Web, Port 3306 for Database).
    

> **Cost Optimization Tip:** While building in India (or elsewhere), remember that NAT Gateways carry an hourly charge (approx. ₹4.5/hr per gateway). For a lab environment, delete them when not in use!

---

## ## Proof of Work

Verification is the soul of engineering. Below are the snapshots from the AWS Console confirming the successful deployment.

### ### 1. Architecture Map

### ### 2. Console Verification

|**Subnet Distribution**|**Route Table Configuration**|
|---|---|
|||

---

## ## Future Roadmap: Terraform Integration

The next phase of this project is to move from "ClickOps" to **Infrastructure as Code (IaC)**. This will allow the entire environment to be version-controlled and deployed in seconds.

- **Phase 1: Resource Scripting** – Translate manual steps into `.tf` files.
    
- **Phase 2: Variables & Outputs** – Make the architecture reusable for different regions.
    
- **Phase 3: Remote State** – Store the architecture state in an S3 Bucket with DynamoDB locking.
    
- **Phase 4: Modularization** – Create a standalone "VPC Module" for future projects.
    

---

## ## About the Author

- **Name:** SoulByte07
    
- **Role:** Aspiring AWS Cloud Engineer
    
- **Focus:** Linux, Networking, and FOSS Security.
    

---
