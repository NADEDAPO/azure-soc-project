# Building an SOC & Honeynet in Azure 


![BMGCOLOR4](https://github.com/user-attachments/assets/a5aca205-e65c-4bc8-ae79-0b9dd07f6222)

## Introduction

For this project we are going to setup our Virtual Machines/Add Users in Active Directory. We will also set up our Log Analytics 
alongside our Sentinel to begin ingesting various malicious log data flowing into our SIEM using the traffic coming from our configured inbound 
rules. Next, we will trigger some alerts and incidents, and do some geo-mapping. Lastly we will run an insecure enviornment for 24 hours
and proceed to harden that envoirnment after those 24 hours using various security controls.



##

🛠 **Requirements, Tools & Controls Implemented:**

- *Azure Virtual Network (Vnet)*
- *Azure Network Security Groups (NSG's/Firewall)*
- *(2) Windows VM's & (1) Linux Virtual Machine*
- *Log Analytics Workspace with Kusto Query Language (KQL) Queries*
- *SQL Server*
- *Azure Key Vault Management*
- *Azure Storage/Blob Management*
- *Microsoft Sentinel for Security Information and Event Management (SIEM)*
- *Microsoft Defender for Cloud to Protect Cloud Resources*
- *Windows Remote Desktop Protocol*
- *Command Line Interface (CLI)*
- *PowerShell for Automation and Configuration Management*
- *Security Controls*
  <details><summary>▶️More</summary>


  NIST 800-53 - (Security and Privacy Controls for Information Systems and Organizations)


  Handbook: https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-53r5.pdf


  NIST 800-61 - (Computer Security Incident Handling Guide)

  
  Handbook: https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf


  Risk Management Framework (RMF):

  
  ![1_aDXOrMWuBNO4pySw5qN4gA](https://github.com/user-attachments/assets/0e84c001-ffd0-48ad-95b6-1ef0667839e8)


  </details>

##
Before Hardening & After Hardening Overview:

![ca47ef83-9025-4309-8ff2-f098bab1784c](https://github.com/user-attachments/assets/fb2cfb7a-4854-4ff0-9f5b-f051e79cc5c9)

##
 Additonal Setup: 
 
  <details><summary>🔽More</summary>
  
  -Adding users via Azure Portal

  
  -Simulating Brute Force Attack

  
  -Insecure & Secure Enviornment Analysis
  
  
  </details>

##
##  📋 Topics Covered


 **KQL Query Overview:**

SecurityEvent (Windows Event Logs)


Syslog (Linux Event Logs)


SecurityAlert (Log Analytics Alerts Triggered)


SecurityIncident (Incidents created by Sentinel)


AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

