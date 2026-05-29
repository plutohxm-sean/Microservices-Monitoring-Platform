# Microservices Monitoring Platform

Multi-region e-commerce monitoring platform built on AWS using VPC Peering, EC2, CloudWatch, and cross-region monitoring architecture.

---

## Overview

This project demonstrates a real-world cloud architecture scenario for monitoring distributed microservices environments across multiple AWS regions.

The infrastructure includes:

* Production Environment in `us-east-1`
* Development Environment in `us-east-1`
* Analytics Environment in `us-west-2`
* Cross-region VPC peering
* EC2-based web and API services
* CloudWatch monitoring and alerting
* Multi-region dashboards

---

## Architecture

```text
US-EAST-1
├── Production VPC (10.1.0.0/16)
│   ├── Web Tier
│   ├── App Tier
│   └── CloudWatch Integration
│
├── Development VPC (10.2.0.0/16)
│   └── Test Environment
│
└── VPC Peering
        │
        ▼

US-WEST-2
└── Analytics VPC (10.3.0.0/16)
    └── Monitoring Dashboard
```

---

## Technologies Used

| Service             | Purpose                 |
| ------------------- | ----------------------- |
| AWS VPC             | Network segmentation    |
| VPC Peering         | Cross-VPC communication |
| EC2                 | Compute instances       |
| Security Groups     | Traffic filtering       |
| CloudWatch          | Monitoring and alerting |
| Node.js + Express   | API services            |
| HTML/CSS/JavaScript | Dashboard frontend      |

---

# Phase 1 - VPC & Networking Setup

## Production VPC

Region: `us-east-1`

Configuration:

```text
CIDR Block: 10.1.0.0/16
Availability Zones: 2
Public Subnets: 2
Private Subnets: 2
NAT Gateway: 1
DNS Hostnames: Enabled
DNS Resolution: Enabled
```

---

## Development VPC

Region: `us-east-1`

Configuration:

```text
CIDR Block: 10.2.0.0/16
Subnet: 10.2.1.0/24
Internet Gateway: Enabled
```

---

## Analytics VPC

Region: `us-west-2`

Configuration:

```text
CIDR Block: 10.3.0.0/16
Subnet: 10.3.1.0/24
Internet Gateway: Enabled
```

---

# Phase 2 - VPC Peering

## Same Region Peering

```text
Production VPC <-> Development VPC
```

---

## Cross Region Peering

```text
Production VPC <-> Analytics VPC
```

---

## Route Tables

### Production VPC

```text
10.2.0.0/16 -> Prod-Dev-Peering
10.3.0.0/16 -> Prod-Analytics-Peering
```

### Development VPC

```text
10.1.0.0/16 -> Prod-Dev-Peering
```

### Analytics VPC

```text
10.1.0.0/16 -> Prod-Analytics-Peering
```

---

# Phase 3 - Security Groups

## Production Web Security Group

Allowed inbound traffic:

```text
HTTP 80      -> 0.0.0.0/0
HTTPS 443    -> 0.0.0.0/0
SSH 22       -> 10.1.0.0/16
```

---

## Production App Security Group

Allowed inbound traffic:

```text
TCP 8080     -> prod-web-sg
TCP 8080     -> 10.2.0.0/16
SSH 22       -> 10.1.0.0/16
```

---

## Development Security Group

```text
HTTP 80      -> 10.1.0.0/16
TCP 8080     -> 10.1.0.0/16
SSH 22       -> 10.1.0.0/16
```

---

## Analytics Security Group

```text
TCP 3000     -> 10.1.0.0/16
SSH 22       -> 10.1.0.0/16
```

---

# Phase 4 - EC2 Deployment

## Production Web Server

Configuration:

```text
AMI: Amazon Linux 2023
Type: t3.micro
Subnet: Public
Public IP: Enabled
```

Installed services:

```text
Apache HTTP Server
Node.js
CloudWatch Agent
Express API
```

Features:

* Web monitoring dashboard
* API integration testing
* CloudWatch metrics testing
* Real-time monitoring simulation

---

## Production App Server

Configuration:

```text
AMI: Amazon Linux 2023
Type: t3.micro
Subnet: Private
Public IP: Disabled
```

Features:

* REST API service
* Health check endpoint
* Order management simulation
* Internal microservice communication

---

## Development Server

Configuration:

```text
AMI: Amazon Linux 2023
Type: t3.micro
Subnet: Public
```

Features:

* Development API
* Test environment
* Beta feature simulation
* Peering validation

---

## Analytics Dashboard Server

Region: `us-west-2`

Configuration:

```text
AMI: Amazon Linux 2023
Type: t3.micro
Port: 3000
```

Features:

* Cross-region dashboard
* Metrics visualization
* Connectivity testing
* Simulated analytics system

---

# Phase 5 - CloudWatch Monitoring

## CloudWatch Alarms

### Production CPU Alarm

```text
Metric: CPUUtilization
Threshold: > 80%
Period: 5 Minutes
```

---

## Network Traffic Alarm

```text
Metric: NetworkIn
Threshold: > 1MB
```

---

## Dashboard Widgets

Included metrics:

* CPU Utilization
* Network Traffic
* Cross-region Metrics
* Application Logs
* Custom Metrics

---

# Testing

## Connectivity Testing

### Test Production to Development

```bash
ping 10.2.1.x
telnet 10.2.1.x 8080
```

### Test Production to Analytics

```bash
ping 10.3.1.x
telnet 10.3.1.x 3000
```

---

## CloudWatch Validation

Verify:

* EC2 metrics visible
* Custom metrics generated
* Dashboard updating
* Alarm notifications triggered

---

# Common Troubleshooting

## VPC Peering Issues

Possible causes:

* Route table misconfiguration
* Security group restrictions
* Peering status not active
* Overlapping CIDR blocks

---

## EC2 Access Issues

Check:

```text
Security Groups
Public IP assignment
Internet Gateway routes
SSH key pair
```

---

## Missing CloudWatch Metrics

Verify:

```text
CloudWatch Agent installed
IAM permissions configured
Application sending metrics
```

---

# Cost Estimation

| Resource        | Estimated Cost |
| --------------- | -------------- |
| 4x EC2 t3.micro | ~$20/month     |
| CloudWatch      | ~$3-5/month    |
| Data Transfer   | ~$1-3/month    |
| Total           | ~$25-30/month  |

---

# Project Validation Checklist

```text
[✓] Multi-region VPC architecture
[✓] VPC peering connections
[✓] EC2 instances deployed
[✓] Security groups configured
[✓] Cross-region communication working
[✓] CloudWatch monitoring active
[✓] Interactive dashboards available
```

---

# Learning Outcomes

This project demonstrates:

* AWS networking architecture
* Multi-region infrastructure
* Cross-VPC communication
* Monitoring and observability
* CloudWatch metrics and alarms
* Security group management
* EC2 deployment automation
* Real-world cloud troubleshooting

---

# Cleanup

To avoid unnecessary AWS charges:

```text
1. Stop or terminate EC2 instances
2. Delete CloudWatch alarms
3. Remove dashboards
4. Delete VPC peering connections
5. Remove unused VPCs
```

---

# Author

Sean / plutohxm

Cloud & Network Engineering Lab Project

Politeknik Elektronika Negeri Surabaya (PENS)
