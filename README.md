# Azure Honeynet: Simulating Real-World Cyber Attacks
![Cloud Honeynet / SOC](https://i.imgur.com/XGkgYwl.png)

## Introduction

 I am proud to present this project, which focuses on building a honeynet in Azure to simulate real-world cyber attacks. 
This project showcases my skills in Microsoft Azure security, incident response, cloud engineering and environment hardening.

## Objective
The main objective of this project was to [set up virtual machines that were intentionally vulnerable](https://github.com/DavidDouglass1/Azure-VM-Setup) in the Azure infrastructure to attract and analyze cyber attacks. This helped me to better understand the tactics and techniques used by attackers, while also showcasing my ability to respond quickly and effectively to any identified issues.

## Technologies, Regulations, and Azure Components Employed:

- Entra ID (formerly called Azure Active Directory)
- Azure Virtual Network (VNet)
- Azure Network Security Group (NSG)
- Virtual Machines (2x Windows, 1x Linux)
- Log Analytics Workspace with Kusto Query Language (KQL) Queries
- Azure Key Vault for Secure Secrets Management
- Azure Storage Account for Data Storage
- Microsoft Sentinel for Security Information and Event Management (SIEM)
- Microsoft Defender for Cloud to Protect Cloud Resources
- Windows Remote Desktop for Remote Access
- Command Line Interface (CLI) for System Management
- PowerShell for Automation and Configuration Management
- [NIST SP 800-53 Revision 5](https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final) for Security Controls
- [NIST SP 800-61 Revision 2](https://www.nist.gov/privacy-framework/nist-sp-800-61) for Incident Handling Guidance

## Methodology

- <b>*Creating the honeynet*</b>: I began by [deploying multiple vulnerable virtual machines](https://github.com/DavidDouglass1/Azure-VM-Setup) in Azure, simulating an unsecure environment.

- <b>*Monitoring and analysis*</b>: Azure was configured to ingest log sources from various resources into a log analytics workspace. Microsoft Sentinel was then used to build attack maps, trigger alerts, and create incidents based on the collected data.

- <b>*Security metrics measurement*</b>: After verifying all logs were being properly sent to the correct workspace I observed the environment for 24 hours, recording key security metrics while it was unsecure. This provided a baseline to compare against after implementing remediation measures.

- <b>*Incident response and remediation*</b>: After addressing the incidents and identifying vulnerabilities, I began the process of hardening the environment by applying security best practices and Azure-specific recommendations.

- <b>*Post-remediation analysis*</b>: I re-observed the environment for another 24 hours to measure security metrics again, comparing the results with the initial baseline.


## Architecture Prior to Implementing Hardening Measures and Security Controls
![Architecture Diagram](https://i.imgur.com/Sfsf6HM.png)

<b>Before Hardening Measures and Security Controls:</b>

- In the "BEFORE" stage of the project, all resources were initially deployed with public exposure to the internet. This setup was intentionally unsecure to attract potential cyber attackers and observe their tactics. The Virtual Machines had both their Network Security Groups (NSGs) and built-in firewalls wide open, allowing unrestricted access from any source. Additionally, all other resources, such as storage accounts and databases, were deployed with public endpoints visible to the internet, without utilizing any Private Endpoints for added security.

## Architecture After Implementing Hardening Measures and Security Controls
![Architecture Diagram](https://i.imgur.com/aeqOCFN.png)
 <b>For the "AFTER" stage, I implemented a series of hardening measures and security controls to improve the environment's overall security posture. These improvements included:</b>

- <b>Network Security Groups (NSGs)</b>: I hardened the NSGs by blocking all inbound and outbound traffic, with the sole exception of my own public IP address. This ensured that only authorized traffic from a trusted source was allowed to access the virtual machines.

- <b>Built-in Firewalls</b>: I configured the built-in firewalls on the virtual machines to restrict access and protect the resources from unauthorized connections. This step involved fine-tuning the firewall rules based on the specific requirements of each VM, thereby minimizing the potential attack surface.

- <b>Private Endpoints</b>: To enhance the security of other Azure resources, I replaced the public endpoints with Private Endpoints. This ensured that access to sensitive resources, such as storage accounts and databases, was limited to the virtual network and not exposed to the public internet. As a result, these resources were protected from unauthorized access and potential attacks.

By comparing the security metrics before and after implementing these hardening measures and security controls, I was able to demonstrate the effectiveness of each step in improving the overall security posture of the Azure environment.

## Attack Maps Before Hardening / Security Controls
<br />


> <b>NOTE: The attack maps were generated by extracting data from a workbook utilizing pre-built [KQL .JSON](Sentinel Maps (JSON)/linux-ssh-fail.json) map files. These files provided a structured representation of the attack patterns and their associated data, 
enabling the creation of visualizations that effectively illustrated the cyber threats and their impact on the system.</b>


 <br />
 <br />
 
- <b>This attack map demonstrates the consequences of leaving the Network Security Group (NSG) open, as it allowed for malicious traffic to flow unimpeded. This visualization underscores the importance of implementing proper security measures, such as restricting NSG rules, to prevent unauthorized access and minimize potential threats.</b>


![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/WFDWYNc.png)<br>

 <br />
 <br />
 
 - <b>This attack map highlights the numerous syslog authentication failures experienced by the Linux server I deployed, indicating that unauthorized access attempts were made from outisde. This serves as a reminder of the importance of securing Linux servers with strong authentication mechanisms and monitoring system logs for signs of intrusion attempts.</b>
 
![Linux Syslog Auth Failures](https://i.imgur.com/OWQBmDH.png)<br>

 <br />
 <br />
 
 - <b>This attack map showcases numerous RDP and SMB failures, illustrating the persistent attempts by potential attackers to exploit these protocols. The visualization emphasizes the need for securing remote access and file sharing services to protect against unauthorized access and potential cyber threats.</b>
 
![Windows RDP/SMB Auth Failures](https://i.imgur.com/r3B7xqp.png)<br>

 <br />
 <br />

## Attack Maps After Hardening / Security Controls

> All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.

 <br />
 <br />
 
## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our unsecure environment for 24 hours:

Start Time 2024-12-29 15:22:34<br>
Stop Time 2024-12-30 17:22:34

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent (Windows VM)            | 21983
| Syslog (Linux VM)                   | 31992
| SecurityAlert (Microsoft Defender for Cloud            | 0
| SecurityIncident (Sentinel Incidents)        | 218
| NSG Inbound Malicious Flows Allowed | 3611



## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-03-18 15:37
Stop Time	2023-03-19 15:37


| Metric                   | Count
| ------------------------ | -----
| SecurityEvent (Windows VM)            | 783
| Syslog (Linux VM)                   | 23
| SecurityAlert (Microsoft Defender for Cloud            | 0
| SecurityIncident (Sentinel Incidents)        | 0
| NSG Inbound Malicious Flows Allowed | 0

## Conclusion

In conclusion, I set up a compact, but effective honeynet using Microsoft Azure's robust cloud infrastructure. Microsoft Sentinel was then utilized to trigger alerts and generate incidents based on the logs ingested from the implemented watch lists. Baseline metrics were recorded in the unprotected environment before the implementation of any security controls. Following this, a range of security measures were enforced to fortify the network against potential threats. Upon implementation of these controls, another set of measurements was taken.

The comparison of pre- and post-implementation metrics demonstrated a significant reduction in security events and incidents, which highlights the effectiveness of the enforced security controls.
It's important to mention that if the network's resources were extensively engaged by regular users, it's plausible that a higher number of security events and alerts could have been produced within the 24-hour timeframe post-security control implementation.
