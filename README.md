# Building a SOC + Honeynet in Azure (Live Traffic)
![Untitled drawing (1)](https://github.com/Jlope677/Azure-Honeynet-SOC-Project/assets/119469976/c1d85b09-2267-421d-b661-6b1e140d76a3)


## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
NSG Malicious Allowed in
![before nsg-malicious-allowed-in](https://github.com/Jlope677/Azure-Honeynet-SOC-Project/assets/119469976/a9207d1a-2e22-48c7-8e2f-615edb6b5fe2)
<br>
Linux Syslog Auth Failures
![before linux-ssh-auth-fail](https://github.com/Jlope677/Azure-Honeynet-SOC-Project/assets/119469976/0f43ce45-7ae4-4fcd-927f-d1dfb98c1a15)
<br>
Windows RDP/SMB Auth Failures
![before windows-rdp-auth](https://github.com/Jlope677/Azure-Honeynet-SOC-Project/assets/119469976/453f1777-ba74-4470-9e55-95f85dc27eb6)
<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-12-30 18:41:37
Stop Time 2023-12-31  18:41:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 172875
| Syslog                   | 6566
| SecurityAlert            | 2
| SecurityIncident         | 264
| AzureNetworkAnalytics_CL | 67902

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-1-3 7:36:46 
Stop Time	2024-1-4  7:36:46

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 0
| Syslog                   | 4
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
