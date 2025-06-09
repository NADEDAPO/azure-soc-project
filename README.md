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
<details><summary>🔽Log Analytics Explained</summary>


  
The first thing we want to do is create our "LAW" or Log Analytic Workspace. This will be the center of everything that 
we are going to implement within Microsoft Azure. So if we could think of our SOC like the human anatomy, this would be the heart or chest. 
When opening Azure, click the search bar and type in "Log Analytic Workspace." 
  
![LAW 01](https://github.com/user-attachments/assets/e6ec19f9-7f27-49b7-a398-2b0009884349)

 Next, we want to create a new subscription group. Followed by a new resource group. Lastly lets give our metaphorical "SOC Human" a name. In my case
i'm going to name it "Cyber-ThunderLAW" (Putting LAW on the end, so it's distinctively known that this is our center workspace.)

![LAW 02](https://github.com/user-attachments/assets/1993c219-4658-4eff-bd4d-1c4abd0c0c69)

If setup properly then it should look like the following image, with a green check mark.

![LAW 03](https://github.com/user-attachments/assets/748bcb3c-f08d-4b97-856e-e9e454dfbaef) 

</details>

##

<details><summary>🔽Microsoft Sentinel Explained</summary>

Now we are gonna setup our Microsoft Sentinel. We can think of this as the brain of our "SOC-Human" that picks up all the data 
or feelings/perceptions in the 'heart' & behins to process and make alerts/responses for those feelings. (_We will begin by searching for
Microsoft Sentinel in the searchbar._)

![MS 01](https://github.com/user-attachments/assets/20d7a07d-88d2-4ba0-8661-8581fb3fa21d)

To setup our watchlist, we want to click 'New' under watchlist. This will pull a large database of IP's and other various metric 
data for us to narrow down our malicious flows to a certain region and city if possible.

![MS 02](https://github.com/user-attachments/assets/f79a2188-8c97-420e-874f-ab199471ff9f)

The name and alias can be the same. Just continue to the next page.

![MS 03](https://github.com/user-attachments/assets/5a3fc5ab-0dac-48f1-8468-9ba52b1f55cd)

On the watchlist wizard, select the file type to be 'local' and in this geoip.csv file & change the search key into Network. Continue and 
your watchlist should begin to compile within a few minutes or a few hours.

![MS 04](https://github.com/user-attachments/assets/568c75e8-a3e6-40dd-801f-3b1eae2ea1c3)

Within our Microsoft Sentinel, navigate into the left column under content hub. And click Data Connector. This will link the logs coming from our
Virtual Machines into our Log Analytic Workspace. And our "SOC Human" will be given a 'nervous system' to react to this ingested data.

![MS 05](https://github.com/user-attachments/assets/4206cd6e-699a-4807-8154-f89d8218cc11)

Type. SecurityEvent (Windows Event Logs) and install this connector to collect logs from Windows VM's.

![MS 06](https://github.com/user-attachments/assets/8fe192ac-b3dc-4b20-a3cb-36c1049f39b3)

Type. Syslog (Linux Event Logs) and install this connector to collect logs from Linux VM's.

![MS 07](https://github.com/user-attachments/assets/995cfca1-7fd5-46ea-8fa9-31a15ca79d2d)

</details>

##

<details><summary>🔽Virtual Machines Explained</summary>

  
Time for us to setup our virtual machines after which we will expose them to the internet. Search for Virtual Machines
in the search bar and create 2 Windows Machines.


![VM 01](https://github.com/user-attachments/assets/c2bb3f6c-ca3b-473d-9480-32c671f2f10a)

Select the same resources that were made when creating your (LAW) Log Analytic Workspace. And select your specs.

![VM 02](https://github.com/user-attachments/assets/a76e69fe-6901-4228-a8aa-a75b49a28ba1)

Leave your inbound port as RDP 3389, and select login credentials. 

![VM 03](https://github.com/user-attachments/assets/8bd09cc7-4a2d-43a7-822c-59b696ab3db6)

![VM 04](https://github.com/user-attachments/assets/630f4a46-70b7-49ef-99b7-282da4b2c198)

Create a virtual network and subnet mask.

![VM 05](https://github.com/user-attachments/assets/1a90f675-2b8f-496e-be16-2422d3627bf7)

![VM 06](https://github.com/user-attachments/assets/e66fd576-1f64-4da3-99c1-ff883d1baa50)

![VM 07](https://github.com/user-attachments/assets/f81831aa-a9f5-4e50-bc2d-02775960c2d0)

Your port number for your linux machine should be SSH-22.

![VM 08](https://github.com/user-attachments/assets/a8d109dd-e8a7-4489-9b38-ef7df0c96dc6)

![VM 09](https://github.com/user-attachments/assets/ae1a9138-7a47-4694-8e0d-26adccfc81fb)

![VM 10](https://github.com/user-attachments/assets/754a790c-93ee-4630-b2ac-50ce5b2c68f4)

</details>

##

<details><summary>🔽Network Security Group Explained</summary>
  
Search for Network Security Group or NSG in the search bar.

![NSG 01](https://github.com/user-attachments/assets/512ae95d-4088-4065-9a83-7b20347abec9)

Select the NSG from the Virtual Machine you created.

![NSG 02](https://github.com/user-attachments/assets/2bc16dbc-5651-4314-9d91-5defdd09c96e)

Under settings go to inbound security rules and update the rules for your inbound ports. Allow all flows
to come in. And set the port priority to 100. Rename your new rule to be called "Danger_AllowAny" and delete
the other previous rule.

![NSG 03](https://github.com/user-attachments/assets/a1d62ab6-4c7d-40c7-a042-6359fca2bbf2)

![NSG 04](https://github.com/user-attachments/assets/6ffc5728-979f-474f-a4b1-b43193239406)

</details>

##

![DFC 01](https://github.com/user-attachments/assets/3212cda8-d72b-4626-b461-46ea8612f55b)


![DFC 02](https://github.com/user-attachments/assets/d4b93dab-d758-45ac-a918-d9551289a731)


![DFC 03](https://github.com/user-attachments/assets/10e7452f-b781-4357-889d-3d0cd2d36efa)


![DFC 04](https://github.com/user-attachments/assets/9f958997-3987-4cc3-ac52-e81373823782)

![DFC 05](https://github.com/user-attachments/assets/a191f1c3-848b-49c1-af4e-c45aea18656a)


![DFC 06](https://github.com/user-attachments/assets/3852a876-16e4-4fda-8ca1-3215cb0b876f)


![DFC 07](https://github.com/user-attachments/assets/75ecbf50-82b5-48d2-a426-13ddcf11eb35)


![DFC 08](https://github.com/user-attachments/assets/12d22d1e-0beb-44ea-bed4-ca1e8708122b)








![BLOB 01](https://github.com/user-attachments/assets/4655176f-ce6a-4723-b241-784570da53c2)


![BLOB 02](https://github.com/user-attachments/assets/98f23b0d-192f-4389-9f23-25eeeb22db17)


![BLOB 03](https://github.com/user-attachments/assets/98ac2927-fb12-4f97-ace3-0dd4cf00a302)


![BLOB 04](https://github.com/user-attachments/assets/cb1bb877-f28a-44b4-b5b1-660016c31be7)


![BLOB 05](https://github.com/user-attachments/assets/17bb7ee2-81f0-447d-b35a-88ab0ac9f5e7)



![KV 01](https://github.com/user-attachments/assets/691f89d0-d9d4-4bc2-8c7a-45866a930e64)

![KV 02](https://github.com/user-attachments/assets/db72fcd6-8e33-42ad-b85d-70ba647847dd)


![KV 03](https://github.com/user-attachments/assets/34fc139f-7af3-4304-9b76-50c8c7d9103d)

![KV 04](https://github.com/user-attachments/assets/8f9539e4-12eb-45a0-aa59-675a200c9238)



![MEI 01](https://github.com/user-attachments/assets/bb2c89b8-2e35-4d45-8a60-f4aa8acfbcb3)


![MEI 02](https://github.com/user-attachments/assets/3917f2a5-394b-4c70-bacf-114a454903ea)

![MEI 03](https://github.com/user-attachments/assets/75d18070-5e56-450c-9737-14320b6378d6)


![MEI 04](https://github.com/user-attachments/assets/b342af8e-68a7-4c42-977e-bfd868ad393e)

![MEI 05](https://github.com/user-attachments/assets/9d58c765-872d-49d9-b908-70d2aec6cd53)

![MEI 06](https://github.com/user-attachments/assets/3f342b8f-062f-4f34-a61f-c57402f6a6ad)


![MEI 07](https://github.com/user-attachments/assets/d61aa906-5610-4fa8-8ed7-241cafd7df7e)

![MEI 08](https://github.com/user-attachments/assets/d16c0dab-be10-489f-b6f2-fde72f03ca0d)


![MEI 09](https://github.com/user-attachments/assets/0a3b9bb1-011f-4bdd-9ce9-f4df90e3e983)















