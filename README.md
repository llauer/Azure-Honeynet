# Building a SOC + Honeynet in Azure (Live Traffic)


![Cloud Honeynet / SOC](https://github.com/user-attachments/assets/d1b524f0-fead-42de-a8db-a254a3ea9257)


## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls

![Architecture Diagram](https://github.com/user-attachments/assets/2baf5fe2-b9e6-4882-8bf9-552ad018ffa5)


## Architecture After Hardening / Security Controls

![Architecture Diagram](https://github.com/user-attachments/assets/59ffa4a0-64f4-47de-a101-caafe6e0b241)


The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, and exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources were deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic except my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint


## Attack Maps Before Hardening / Security Controls

![NSG Allowed Inbound Malicious Flows](https://github.com/user-attachments/assets/104170a6-3bfc-4afd-b296-af5486d86580)<br>

![Linux Syslog Auth Failures](https://github.com/user-attachments/assets/86490cff-2405-4612-94a2-73418ee31811)<br>


![Windows RDP/SMB Auth Failures](https://github.com/user-attachments/assets/661bd572-14d5-45eb-829c-9147f56f1ff9)<br>


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
2024-09-16T04:05:06.8930243Z
2024-09-17T04:05:06.8930243Z

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 105830
| Syslog                   | 3553
| SecurityAlert            | 4
| SecurityIncident         | 168
| AzureNetworkAnalytics_CL | 103

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
2024-09-18T16:20:33.7197311Z
2024-09-19T16:20:33.7197311Z

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 0
| Syslog                   | 4
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure, and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
