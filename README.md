# Azure Enterprise Secure Lab

## Overview

This project demonstrates the design and implementation of a secure Azure enterprise environment using security-first architecture principles.

The lab implements:

- Hub-and-Spoke network topology  
- Role-Based Access Control (RBAC)  
- Azure Policy enforcement  
- Microsoft Defender for Cloud  
- Secure Linux VM deployment  
- Centralized logging with Log Analytics  
- KQL-based governance auditing  

The focus is on preventive, detective, and governance security controls aligned with Azure best practices.

---

# Architecture

The environment follows a Hub-and-Spoke model:

## Hub VNet
- AzureFirewallSubnet
- ManagementSubnet

## Spoke VNet
- AppSubnet
- AKSSubnet

- Bidirectional VNet Peering  
- Network Security Groups applied at subnet level  
- Zero public exposure for workloads  

---

# Phase 1 – Secure Networking Foundation

## Resource Groups

Separate resource groups were created for logical isolation and governance.

## Virtual Networks

- Hub VNet for shared services  
- Spoke VNet for application workloads  

## Network Security Groups

Inbound rules configured:
- Restricted SSH access  
- Internal HTTP allowed  
- Default Deny All Inbound  

---

# Phase 2 – Identity & Governance

## RBAC Design

Security groups created in Entra ID:

- Cloud Admins → Owner (Subscription scope)
- App Developers → Contributor (Resource Group scope)
- Security Readers → Reader + Security Reader

Principle of least privilege enforced.

## Azure Policy

Policy enforced:

> Network interfaces should not have public IPs.

Compliance verified via policy evaluation.

## Defender for Cloud

- Microsoft Defender for Servers enabled  
- No critical findings  
- Secure configuration baseline validated  

---

# Phase 3 – Secure Virtual Machine

A hardened Linux VM was deployed with:

- SSH key authentication only  
- Password login disabled  
- No public IP address  
- No public inbound ports  
- Encrypted OS disk (SSE with PMK)  
- Deployed within segmented subnet  

---

# Phase 4 – Monitoring & Logging

## Log Analytics Workspace

Centralized logging workspace deployed.

## Activity Log Export

Administrative, Security, and Policy logs exported to Log Analytics.

## KQL Governance Query

```kql
AzureActivity
| where OperationNameValue contains "write"
| where ActivityStatusValue == "Success"
