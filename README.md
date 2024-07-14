# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I set up a small honeynet in Azure and gathered logs from different resources into a Log Analytics workspace. Microsoft Sentinel then utilized this workspace to create attack maps, generate alerts, and manage incidents. Initially, I measured several security metrics in the unsecured environment for a 24-hour period. Afterward, I implemented various security controls to fortify the environment, measured the metrics for another 24 hours, and documented the results below. The metrics to be presented are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![unsecured network](https://github.com/user-attachments/assets/d9c80b7a-d12e-4cac-ab05-d273d33f2f0a)


## Architecture After Hardening / Security Controls
![secured network](https://github.com/user-attachments/assets/aa692eb8-c2cc-4dd5-8e4c-c610865bddad)


The architecture of the mini honeynet in Azure includes the following components:

Virtual Network (VNet)
Network Security Group (NSG)
Virtual Machines (2 Windows, 1 Linux)
Log Analytics Workspace
Azure Key Vault
Azure Storage Account
Microsoft Sentinel

Initially, all resources were deployed and exposed to the internet, with the Virtual Machines' Network Security Groups and built-in firewalls left wide open. Other resources were also deployed with public endpoints visible to the Internet, with no utilization of Private Endpoints.

After the security enhancements, Network Security Groups were tightened to block all traffic except from my admin workstation, and other resources were shielded by their built-in firewalls as well as by Private Endpoints.

## Attack Maps Before Hardening / Security Controls
![NSG allowed in](https://github.com/user-attachments/assets/598c406a-7368-4b14-9539-fd49d7f3e2a3)
<br>
![Linux Syslog Auth Failures](https://github.com/user-attachments/assets/2dc23d12-a21f-472c-8118-bfb45442ab7c)
<br>
![Windows RDP/SMB Auth Failures](https://github.com/user-attachments/assets/d396b20c-0bf7-4141-ada3-c93dec6997c3)
<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-07-11 17:12:07
Stop Time 2024-07-12 17:12:07

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 52751
| Syslog                   | 2572
| SecurityAlert            | 0
| SecurityIncident         | 263
| AzureNetworkAnalytics_CL | 1951

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we applied security controls:
Start Time 2024-07-12 21:07:15
Stop Time	2024-07-13 21:07:15

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 9628
| Syslog                   | 44
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was created in Microsoft Azure, with integrated log sources connected to a Log Analytics workspace. Microsoft Sentinel was utilized to generate alerts and create incidents based on the incoming logs. Metrics were collected in the insecure environment both before and after applying security controls. Significantly, the number of security events and incidents was markedly reduced following the application of security measures, highlighting the effectiveness of these controls. 

It is important to mention that if the network resources had been heavily accessed by regular users, a higher number of security events and alerts might have been recorded in the 24-hour period after implementing the security controls.
