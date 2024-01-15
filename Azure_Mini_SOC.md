# Building a SOC + Honeynet in Azure (Live Traffic)
![image](https://github.com/jwinn91/Cloud_Projects/assets/103306552/16b317d2-82f3-4fa5-b431-d2ae5bd5d13b)

## Introduction

In this project, I built a mini honeynet in Azure and ingested log sources from various resources into Azures Log Analytics workspace. Using KQL queries and Microsoft Sentinel to build attack maps, I was able to geo-locate different alerts and log incidents that I later worked through and remediated using NIST 800-61 as my guideline. I measured and collected security metrics in the insecure environment for 24 hours which shows metrics for Windows security events as well as Syslog for Linux that shows logon attempts from different areas around the globe as depicted on the live attack maps.

After running the insecure environment for 24 hours, I went back and hardened my environment by going through and remediating vulnerable network security groups and exposed ports as well as exposed protocols on my virtual Machines as well as locking down the subnet they were in. I studied the effects of it for another 24 hours to see how the metrics changed, and what alerts, if any were left to be triggered by Defender for Cloud, and Microsoft Sentinel. The reporting logs and events are as follows:


- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into honeynet)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

## Architecture Before Hardening / Security Controls
<img width="1021" alt="image" src="https://github.com/jwinn91/Cloud_Projects/assets/103306552/22f52103-b58e-46b2-82fa-8ee993209f86">


## Architecture After Hardening / Security Controls
<img width="1040" alt="image" src="https://github.com/jwinn91/Cloud_Projects/assets/103306552/5b8d637c-9be9-4ef1-91ef-27491590e760">



For the "BEFORE" metrics, all resources were originally deployed, and exposed to the internet through ports and protocols such as RDP and SSH with Any/Any rules set in the virtual Machines so that it would not limit where the traffic would be coming from. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources such as the key vault for my Azure tenant as well as the blob storage were also exposed.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic except any traffic that came from my workstation. To effectively harden the environment after the collection period, I went through my environment and locked down the open ports and protocols on my virtual machines, as well as my SQL Server that were internet exposed. I also spent time working on other environmental vulnerabilities in my environment using Microsoft Defender for Cloud regulatory compliance, and NIST 800-53 R.5 framework that I integrated in Azure to effectively manage the current insecure issues with my virtual machines and SQL server, as well as other areas of attention within my cloud environment.

## Attack Maps Before Hardening / Security Controls
[Malicious NSG Allowed In]<img width="1025" alt="image" src="https://github.com/jwinn91/Cloud_Projects/assets/103306552/e37b10d3-a946-4e79-be46-537c4ce78d24"><br>
[Linux Syslog Auth Failures]![image](https://github.com/jwinn91/Cloud_Projects/assets/103306552/992e5294-623d-4433-a17e-c5c401d2a1a1)<br>
[Windows RDP/SMB Auth Failures]![image](https://github.com/jwinn91/Cloud_Projects/assets/103306552/359cfdc9-a3ac-4e60-ba99-f57934e97a13)<br>
[My SQL Auth Failures]![image](https://github.com/jwinn91/Cloud_Projects/assets/103306552/2d157c56-f247-4368-8cc1-ee6d5b676a10)<br>
[KQL query showing Failed log-ons from attackers on Windows VM's using different Usernames]<img width="956" alt="image" src="https://github.com/jwinn91/Cloud_Projects/assets/103306552/2f1b9bef-2455-4bca-9701-04895fb6af12">



## Metrics Before Hardening / Security Controls

The following table shows the metrics I measured in my insecure environment for 24 hours:

Start Time 2023-10-01T20:08:09.35

Stop Time 2023-10-02T20:08:09.35

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 9496
| Syslog                   | 1165
| SecurityAlert            | 4
| SecurityIncident         | 91
| AzureNetworkAnalytics_CL | 2437




## Metrics After Hardening / Security Controls

```All map queries returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:

Start Time 2023-10-08T12:26:03

Stop Time	2023-10-09T12:26:03

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 2421
| Syslog                   | 5
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure, and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents was drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24 hours following the implementation of the security controls.
