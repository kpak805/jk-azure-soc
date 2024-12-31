# Building a Mini SOC and Honeynet in Azure

## Introduction

In this project, I build a mini honeynet in Azure and collect log sources from various resources into a Log Analytics workspace. It is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![image](https://github.com/user-attachments/assets/a6a26e17-7af3-436b-a199-4dbb3a20edf3)

## Architecture After Hardening / Security Controls
![Architecture After Hardening](https://github.com/user-attachments/assets/42be55d0-5b97-43a5-a534-67452d5e0948)


The honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all virtual machines were deployed completely exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

##Before Hardening / Security Controls
![nsg-auth-allowed-in](https://github.com/user-attachments/assets/3d1f2b7f-78ad-4402-93eb-3197c088fb15)
![linux-ssh-auth-fail](https://github.com/user-attachments/assets/d3fa1d38-31cf-41e5-82cc-e51ad4c67134)
![Win-rdp-auth-fail](https://github.com/user-attachments/assets/62d5cc4d-39c2-47c4-80cc-d142cec32037)


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-12-26 21:00
Stop Time 2024-12-27 21:00 

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 33875
| Syslog                   | 10803
| SecurityAlert            | 6
| SecurityIncident         | 164
| AzureNetworkAnalytics_CL | 3338

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-12-29 21:00
Stop Time	2024-12-30 21:00 

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 9380
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was used to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
