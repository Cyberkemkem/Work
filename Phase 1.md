**Azure IAM & Security Portfolio**
**Phase 1 – Identity & Access Management (IAM)**

This repository documents Phase 1 of an Azure security project focused on building a secure identity and access foundation using Microsoft Entra ID and Azure RBAC.

The goal of this phase was to ensure that access control, MFA enforcement, and privilege management were properly implemented before deploying any network or application resources.

**Project Objectives**

Establish identity as the primary security boundary

Enforce least-privilege access using Azure RBAC

Implement Multi-Factor Authentication (MFA) using Conditional Access

Separate administrative access from standard user access

Prevent tenant lockout through break-glass account design

 **Scope**

Phase 1 covers:

Azure tenant and subscription hygiene

User and security group design

Role-Based Access Control (RBAC)

Conditional Access policies

Break-glass (emergency access) configuration

Validation and testing

🏗 **Tenant & Subscription Setup**

A clean Azure tenant was created and verified

A Pay-As-You-Go subscription was enabled to support enterprise services

Audit logs and sign-in logs were enabled for visibility

A single primary account was kept as Owner at the subscription level

 **Security Group Design**

All permissions are assigned through security groups, not directly to users.

Group Name	Purpose
SG-AZ-Global-Admins	Administrative access with strict MFA
SG-AZ-App-Users	Standard application users
SG-AZ-DevOps	Resource deployment and management
SG-AZ-Security-Readers	Read-only security monitoring
SG-AZ-BreakGlass	Emergency access accounts


This approach simplifies access management and supports scalability.

 **User Management**

To simulate a real enterprise environment, internal cloud-only users were created using role-based naming:

User	Description	Group
admin.user	Administrative account	SG-AZ-Global-Admins
app.user	Standard application user	SG-AZ-App-Users
sec.user	Security monitoring account	SG-AZ-Security-Readers
Breakglass emergency account

<img width="1851" height="822" alt="user phase 1" src="https://github.com/user-attachments/assets/df19a5f3-fc8c-4e8c-a517-aade65ce1b53" />

 **Role-Based Access Control (RBAC)**

RBAC was applied at the subscription level using least-privilege principles.

Role	Assigned To	Purpose
Owner	Primary admin account	Full control, limited to one account
Contributor	SG-AZ-DevOps	Manage resources without role assignment rights
Reader	SG-AZ-Security-Readers	View-only access for monitoring

Note: Due to Azure Portal role picker limitations, the Contributor role was assigned using Azure CLI, reflecting real-world operational troubleshooting.
<img width="1900" height="910" alt="access control 1" src="https://github.com/user-attachments/assets/7a2299b7-118f-49c2-8935-d778c4c18395" />

<img width="1898" height="1013" alt="access control iam cloudshell" src="https://github.com/user-attachments/assets/829fbc59-c821-4287-9812-8fdd75cd53cb" />

<img width="1917" height="890" alt="access control iam complete" src="https://github.com/user-attachments/assets/9b77cc94-728c-428a-a8be-51bfb0e3c43b" />



 **Conditional Access & MFA**

Azure Security Defaults were disabled and replaced with custom Conditional Access policies for better control.

Admin MFA Policy

Policy: CA-Admins-Require-MFA

Target: SG-AZ-Global-Admins

Apps: All cloud apps

Control: Require MFA

Status: Enabled

<img width="1912" height="931" alt="mfa policy 1" src="https://github.com/user-attachments/assets/683b1297-7602-42cc-9c34-2cf69a8c53d7" />


Application User MFA Policy

Policy: CA-AppUsers-Require-MFA

Target: SG-AZ-App-Users

Apps: All cloud apps

Control: Require MFA

Status: Enabled
<img width="1807" height="941" alt="mfa policy 2" src="https://github.com/user-attachments/assets/afdfa2ad-c537-4af3-a0ea-b87230fb80ff" />


Admins and standard users are handled by separate policies to reduce risk.

 **Break-Glass (Emergency Access)**

To prevent tenant lockout scenarios:

Dedicated break-glass admin accounts were created

MFA was intentionally not enforced

Accounts were grouped under SG-AZ-BreakGlass

The group was excluded from all Conditional Access policies

These accounts are secured and not used for daily operations.

 **Validation & Testing**

The configuration was validated by:

Testing MFA enforcement for admin and standard users

Reviewing Conditional Access results in sign-in logs

Verifying RBAC permissions at the subscription level

Confirming audit and sign-in logs were recording events correctly

<img width="1607" height="933" alt="mfa 1 worked" src="https://github.com/user-attachments/assets/37313b40-c763-4a83-8330-70e9212b11d2" />

<img width="1496" height="811" alt="mfa 2 worked" src="https://github.com/user-attachments/assets/1fe4aa03-0c9f-414d-8357-34bb0d4895ad" />

<img width="1900" height="887" alt="sign in logs mfa success" src="https://github.com/user-attachments/assets/653f9616-ae32-4ec9-9a9a-b5bbabbaf9b3" />

**Outcome**

At the end of Phase 1:

Identity is the primary security control

MFA is enforced correctly and consistently

Privileged access is tightly restricted

Emergency access is protected

The environment is ready for secure network deployment

🧠 Key Skills Demonstrated

Azure IAM & Entra ID

Conditional Access & MFA

RBAC & least privilege
 **Next Phase**
**Phase 2 – Network & Subnet Design (Zero Trust Foundation)
Overview**

Phase 2 focused on designing and implementing a secure Azure network foundation before deploying any compute or application resources.

The goal was to enforce network segmentation, least-privilege traffic flow, and prepare the environment for WAF, Firewall, and DDoS protection in later phases.

No virtual machines or services were deployed in this phase — only core networking components.
Objectives

Create a secure Virtual Network (VNet)

Segment workloads into UI, API, and Backend subnets

Enforce east-west traffic control using NSGs

Prevent unnecessary internet exposure

Align with Zero Trust networking principles

**Virtual Network Design**

A single core Virtual Network was created to host all application subnets.

**VNet Details**

Name: vnet-az-sec-core

Resource Group: rg-az-sec-network

Region: West Europe

Address Space: 10.0.0.0/16

Security services such as Azure Firewall, DDoS Protection, and Bastion were intentionally disabled at this stage and will be enabled in later phases.
<img width="1916" height="905" alt="vnet created" src="https://github.com/user-attachments/assets/a4ee46ef-e992-4d69-af9b-0d29f81a5d13" />

**Subnet Segmentation**

The VNet was segmented into dedicated subnets to isolate application tiers.

**Subnet Name**	**Address Range**	**Purpose**
snet-ui	        10.0.1.0/24	     Frontend / UI tier
snet-api	      10.0.2.0/24    	Application / API tier
snet-backend	  10.0.3.0/24	    Backend / data tier
default       	10.0.0.0/24	    Unused (left untouched)

Each subnet is designed to host only its intended workload type.
<img width="1913" height="882" alt="subnet created for project 2" src="https://github.com/user-attachments/assets/f135ed73-72f1-429f-a82c-eeb022ef5bd9" /> 

**Network Security Groups (NSGs)**

A one-to-one NSG-to-subnet model was used to keep security boundaries clear.

**NSG**	**Attached Subnet**
nsg-ui	  snet-ui
nsg-api 	 snet-api
nsg-backend	 snet-backend

NSGs were applied at the subnet level, not NIC level, to enforce consistent policy.
<img width="1917" height="961" alt="nsg attatched to subnets" src="https://github.com/user-attachments/assets/a91be119-6892-47ed-91af-2e00beb76f1c" />

**Traffic Flow Rules (Zero Trust)**

Explicit inbound rules were created to allow only required traffic between tiers.
All other traffic is implicitly denied by default.

**UI Subnet (nsg-ui)**

Allow HTTP (TCP 80) from Internet (temporary, to be replaced by WAF)

Allow UI → API on TCP 443
<img width="1900" height="957" alt="nsg inbound ui" src="https://github.com/user-attachments/assets/8e1c7d63-5dc5-40ea-af0c-e5702bdd1542" />

**API Subnet (nsg-api)**

Allow UI → API on TCP 443

Allow API → Backend on TCP 1433
<img width="1897" height="922" alt="nsg inbound api" src="https://github.com/user-attachments/assets/2af77264-7f38-46a7-b606-ba24685df875" />

**Backend Subnet (nsg-backend)**

Allow API → Backend on TCP 1433 only

No internet-facing rules

No UI access
<img width="1876" height="883" alt="nsg inbound backend" src="https://github.com/user-attachments/assets/8aed8185-db00-4135-8026-d7afe8bc409b" />

Outbound rules were left at default and will be hardened in later phases.

**Security Principles Applied**

Clear separation of application tiers

Least-privilege network access

Explicit allow rules only where required

Default deny for all other traffic

No direct backend or database exposure

**Validation**

The configuration was validated by:

Confirming NSGs were correctly attached to each subnet

Reviewing inbound rules for each NSG

Ensuring no unintended internet access to API or Backend subnets

**Outcome of Phase 2**

At the end of this phase:

The network is fully segmented

East-west traffic is controlled

The backend tier is isolated

The environment is ready for WAF, Firewall, and DDoS integration

**Key Skills Demonstrated**

Azure Virtual Networking

Subnet design & IP planning

Network Security Groups (NSGs)

Zero Trust networking

Secure traffic flow design

Cloud security architecture

**Phase 3 – Application Gateway & Web Application Firewall (WAF)
Overview**

Phase 3 focused on securing all inbound traffic into the environment using Azure Application Gateway (WAF v2).
The objective was to introduce a single, hardened entry point for the application and remove any direct internet exposure to internal subnets.

This phase establishes the ingress security layer for the architecture.

**Objectives**

Centralize inbound traffic through Azure Application Gateway

Enable Web Application Firewall (WAF) with OWASP protections

Enforce Zero Trust ingress (no direct public access to subnets)

Prepare the environment for HTTPS and advanced protections

**Application Gateway Deployment**
An Azure Application Gateway was deployed with WAF v2 enabled.

**Application Gateway Details**

Name: agw-az-sec-waf

Tier: WAF v2

Resource Group: rg-az-sec-network

Region: West Europe

Availability Zones: 1, 2, 3

Frontend: Public IPv4

Autoscaling: Enabled

A dedicated subnet was created for the gateway, following Azure best practices.
<img width="1913" height="1028" alt="appgw deployed" src="https://github.com/user-attachments/assets/4525587f-91f6-406e-82ef-3cdb5dc5bff1" />

**Dedicated Gateway Subnet**

To isolate the Application Gateway, a separate subnet was created:

Subnet	Address Range	Purpose
snet-appgw	10.0.10.0/24	Application Gateway only

No NSG or route table was attached to this subnet, as required by Azure.
<img width="1910" height="947" alt="appgw subnet created" src="https://github.com/user-attachments/assets/1a65130d-2dc0-4dfe-b1fc-a2172ec90156" />

**Backend Pool & Routing**

A backend pool was configured to represent the UI tier.

Backend pool: bp-ui

Target type: IP address (placeholder)

Purpose: Validate routing and health probe behavior

A basic HTTP listener and routing rule were created:

Listener: listener-http (HTTP :80)

Routing rule: rule-http-ui

Backend settings: http-ui-settings (HTTP, port 80)

At this stage, the backend is intentionally unhealthy because no workload has been deployed yet. This validates that health probes and routing are functioning correctly.
<img width="1757" height="986" alt="routing rule page" src="https://github.com/user-attachments/assets/49cac585-813d-4ec3-b77b-2c2eaad8db2a" />
<img width="1905" height="934" alt="image" src="https://github.com/user-attachments/assets/42216d78-76f3-4a5b-9815-46ca2a8ba971" />

**Web Application Firewall (WAF)**

A custom WAF policy was created and associated with the Application Gateway.

WAF Configuration

Policy name: waf-policy-az-sec

Policy state: Enabled

Policy mode: Prevention

Rule set: OWASP (default managed rules)

The policy was initially created in Detection mode and then explicitly switched to Prevention, ensuring active blocking of malicious traffic.
<img width="1912" height="948" alt="wAF PAGE" src="https://github.com/user-attachments/assets/be15ac62-56e7-4d85-8123-ddfd5f1e9101" />

 **Network Lockdown**
After deploying the Application Gateway, the network was tightened:

Direct internet access to the UI subnet was removed

NSG rules were updated to allow:

Application Gateway → UI subnet

All inbound traffic now follows:

**Internet → WAF → UI → API → Backend**
This enforces Zero Trust ingress and prevents accidental exposure.
<img width="1903" height="968" alt="inbound sec ui appgw" src="https://github.com/user-attachments/assets/1ffd847d-40cf-4def-9c91-75ad13dcdf4b" />

**Validation**

The configuration was validated by:

Confirming Application Gateway status was running

Verifying WAF was enabled in Prevention mode

Confirming routing rules and backend settings

Observing expected backend health warnings (no deployed workload)

Ensuring UI subnet had no direct internet access

**Outcome of Phase 3**

At the end of this phase:

All inbound traffic is centralized and inspected

OWASP Top 10 protections are enforced

Public exposure of internal subnets is eliminated

The environment is ready for HTTPS, Firewall, and DDoS integration

**Key Skills Demonstrated**

Azure Application Gateway (WAF v2)

Web Application Firewall configuration

OWASP managed rules

Secure ingress design

Zero Trust networking

Cloud security architecture
