# SOC-AzureProject
# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

This project involves creating a mini honeynet in Azure and integrating logs from various resources into a Log Analytics workspace. Microsoft Sentinel leverages this data to build attack maps, trigger alerts, and generate incidents. Security metrics were measured in the insecure environment for 24 hours, followed by applying security controls, re-measuring for another 24 hours, and comparing the results. The metrics presented below include:

- SecurityEvent --> Windows Event Logs
- Syslog --> Linux Event Logs
- SecurityAlert --> Log Analytics Alerts Triggered
- SecurityIncident --> Incidents created by Sentinel
- AzureNetworkAnalytics_CL --> Malicious Flows allowed into our honeynet

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

List of the components of the mini honeynet architecture in Azure consists are as follows:

- Network Security Group (NSG)
- Virtual Machines (1 linux vm and 2 windows vms)
- Virtual Network (named Lab-VNet)
- Log Analytics Workspace (name LAW-Cyber-lab)
- Azure Key Vault (name akv-cyberlab)
- Azure Storage Account (named sacyberlab02)
- Microsoft Sentinel (the same with Log Analytic Workspace (LAW-Cyber-Lab))

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![NSG-Malicious-Allowed-in](https://github.com/user-attachments/assets/0fbcaa00-3aed-4993-8b37-a6e233762ec8)

![Linux-Auth-Fail](https://github.com/user-attachments/assets/dc729050-62a7-4431-93bb-1b6c0aafb45a)

![Windows-RDP-Auth-Fail](https://github.com/user-attachments/assets/8d919436-f482-44b5-a510-6f049ea6e4dc)

!![MSSQL-Auth-Fail](https://github.com/user-attachments/assets/e64ba4c8-b7f2-44c2-b9ab-e1932113dcfe)


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-03-15 17:04:29
Stop Time 2023-03-16 17:04:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 25149
| Syslog                   | 16159
| SecurityAlert            | 2
| SecurityIncident         | 187
| AzureNetworkAnalytics_CL | 18176

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-03-18 15:37
Stop Time	2023-03-19 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 1107
| Syslog                   | 4200
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

This is a project where a mini honeynet was deployed in Microsoft Azure, with log sources integrated into a Log Analytics workspace. Microsoft Sentinel was utilized to generate alerts and incidents based on the ingested log data. Metrics were collected in the insecure environment prior to applying security controls and again after implementing these measures. The results demonstrated a significant reduction in security events and incidents, highlighting the effectiveness of the applied controls.

Notably, if the network resources had experienced heavy usage by regular users, the 24-hour period following the implementation of security controls could have resulted in a higher volume of security events and alerts.
