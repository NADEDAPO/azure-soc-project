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


##

The first thing we want to do is create our "LAW" or Log Analytic Workspace. This will be the center of everything that 
we are going to implement within Microsoft Azure. So if we could think of our SOC like the human anatomy, this would be the heart or chest. 
When opening Azure, click the search bar and type in "Log Analytic Workspace." 

![LAW 01](https://github.com/user-attachments/assets/e6ec19f9-7f27-49b7-a398-2b0009884349)

 Next, we want to create a new subscription group. Followed by a new resource group. Lastly lets give our metaphorical "SOC Human" a name. In my case
i'm going to name it "Cyber-ThunderLAW" (Putting LAW on the end, so it's distinctively known that this is our center workspace.)

![LAW 02](https://github.com/user-attachments/assets/1993c219-4658-4eff-bd4d-1c4abd0c0c69)

If setup properly then it should look like the following image, with a green check mark.

![LAW 03](https://github.com/user-attachments/assets/748bcb3c-f08d-4b97-856e-e9e454dfbaef)

##

Now we are gonna setup our Microsoft Sentinel. We can think of this as the brain of our "SOC-Human" that picks up all the data 
or feelings/perceptions in the 'heart' & behins to process and make alerts/responses for those feelings. (_We will begin by searching for
Microsoft Sentinel in the searchbar._)

![MS 01](https://github.com/user-attachments/assets/20d7a07d-88d2-4ba0-8661-8581fb3fa21d)

To setup our watchlist, we want to click 'New' under watchlist. This will pull a large database of IP's and other various metric 
data for us to narrow down our malicious flows to a certain region and city if possible.

![MS 02](https://github.com/user-attachments/assets/f79a2188-8c97-420e-874f-ab199471ff9f)

The name and alias can be the same. Just continue to the next page.

![MS 03](https://github.com/user-attachments/assets/5a3fc5ab-0dac-48f1-8468-9ba52b1f55cd)



![MS 04](https://github.com/user-attachments/assets/568c75e8-a3e6-40dd-801f-3b1eae2ea1c3)


![MS 05](https://github.com/user-attachments/assets/4206cd6e-699a-4807-8154-f89d8218cc11)


![MS 06](https://github.com/user-attachments/assets/8fe192ac-b3dc-4b20-a3cb-36c1049f39b3)


![MS 07](https://github.com/user-attachments/assets/995cfca1-7fd5-46ea-8fa9-31a15ca79d2d)


##

Time for us to setup our virtual machines and expose them to the internet. This is how we will collect malicious flows and route 
them back to our (LAW) so our (Sentinel) can make sense of it all.

