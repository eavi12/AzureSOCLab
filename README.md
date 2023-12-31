# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://github.com/eavi12/AzureSOCLab/assets/58631054/2726711f-09b2-444f-b0e9-acaf80bda9de)

## Introduction

In this project, I built a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://github.com/eavi12/AzureSOCLab/assets/58631054/572e3d48-1314-4960-9557-9ccbf4645bec)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://github.com/eavi12/AzureSOCLab/assets/58631054/4a67ff94-738e-42e3-8805-539ef70b026f)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (1 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://github.com/eavi12/AzureSOCLab/assets/58631054/421847f6-d3e2-4baf-8c1a-a6fca366d6ca)
<br>
![Linux Syslog Auth Failures](https://github.com/eavi12/AzureSOCLab/assets/58631054/6478f886-a88c-49bc-99b2-797ed6f32b4d)
<br>
![Windows RDP/SMB Auth Failures](https://github.com/eavi12/AzureSOCLab/assets/58631054/822db162-4633-4de9-b4d1-1661f7d422f6)
<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-12-18 15:14
Stop Time 2023-12-20 15:14
| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 4014
| Syslog                   | 221
| SecurityAlert            | 1
| SecurityIncident         | 19
| AzureNetworkAnalytics_CL | 6001

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-12-21 14:54
Stop Time	2023-12-22 14:54

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 3547
| Syslog                   | 5
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 24

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
