# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction
In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. Azure Sentinel was used to measured the metrics of an insecure environment over a 24-hour period. Following this phase, security controls were implemented to fortify the virtual environment. Lastly, another 24-hour metric measurement phase was conducted and the results obtained from these endeavors are presented below. The metrics analyzed were:


- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Technologies, Azure Components, and Regulations Employed
- Azure Virtual Network (VNet)
- Azure Network Security Groups (NSG)
- Virtual Machines (1 Windows VM, 1 Linux VM)
- Log Analytics Workspace with Kusto Query Language (KQL) Queries
- Azure Key Vault for Secure Secrets Management
- Azure Storage Account for Data Storage
- Microsoft Sentinel for Security Information and Event Management (SIEM)
- Microsoft Defender for Cloud to Protect Cloud Resources
- Windows Remote Desktop for Remote Access
- Command Line Interface (CLI) for System Management
- PowerShell for Automation and Configuration Management
- NIST SP 800-53 Revision 5 for Security Controls
- NIST SP 800-61 Revision 2 for Incident Handling Guidance

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

During the "AFTER" stage of the project, the environment was hardened and security controls were implemented in order to satisfy NIST SP 800-53 Rev5 SC-7(3) Access Points. These hardening tactics included:

- Network Security Groups (NSGs): NSGs were hardened by blocking all inbound and outbound traffic with the exception of designated public IP addresses that required access to the virtual machines. This ensured that only authorized traffic from a trusted source was allowed to access the virtual machines.

- Built-in Firewalls: Azure's built-in firewalls were configured on the virtual machines to restrict unauthorized access and protect the resources from malicious connections. This step involved fine-tuning the firewall rules based on the service and responsibilities of each VM which mitigated the attack surface bad actors had access to.

- Private Endpoints: To enhance the security of Azure Key Vault and Storage Containers, Public Endpoints were replaced with Private Endpoints. This ensured that access to these sensitive resources was limited to the virtual network and not the public internet.

## Attack Maps Before Hardening / Security Controls

**This attack map shows the traffic allowed by a Network Security Group with all traffic allowed inbound:**
![NSG Before](https://github.com/Lachiecodes/Azure-SOC-Honeypot/assets/138475757/f9f68a20-3ac8-40a6-9ea6-5daeab798151)<br>
<br>
**This attack map shows all the attempts malicious actors attempting to access the Linux virtual machine:**
![Syslog Before](https://github.com/Lachiecodes/Azure-SOC-Honeypot/assets/138475757/3720c2a7-4d96-427a-8421-045f9cd283b0)<br>
<br>
**This attack map shows all the attempts malicious actors attempting to access the Windows virtual machine:**
![Windows RDP Before](https://github.com/Lachiecodes/Azure-SOC-Honeypot/assets/138475757/4ef47359-e728-4958-9c63-dede2abf9173)<br>
<br>
**This attack map shows all the attempts malicious actors attempting to access the SQL server on the windows-vm:**
![Mysql Before](https://github.com/Lachiecodes/Azure-SOC-Honeypot/assets/138475757/781e7951-b01e-47a0-990e-6b0248e2b640)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-09-03 23:26:21 
Stop Time 2023-09-04 23:26:21

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 34593
| Syslog                   | 10921
| SecurityAlert            | 11
| SecurityIncident         | 164
| AzureNetworkAnalytics_CL | 1630

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-09-08 16:51:08 
Stop Time 2023-09-09 16:51:08 

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8848
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness. 

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
