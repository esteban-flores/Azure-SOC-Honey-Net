# Building a SOC + Honeynet in Azure (Live Traffic)

![Honey-SOC](https://github.com/esteban-flores/Azure-SOC-Honey-Net/assets/60724828/44e05698-60f3-434c-ab69-7a2f8c324a6d)

## Introduction

This is the biggest and most intensive project that I have built so far. The goal was to simulate what may happen in a Security Operations Center(SOC).
A mini honeynet was built in Microsoft Azure. Logs were ingested from various resources into Log Analytics workspace(LAW). Queries were written in Kusto Query Language (KQL).
The LAW was then used to help visualize the threats with KQL queires that were used by Microsoft Sentinel.
Sentinel built attack maps, triggered alerts, and created incidents that were later analyzed and worked. 
The project has two environments. One was insecure and the other was more secure because it had been hardened after implementing security controls. 
They were both run for 24 hours and five metrics were used to determine the effectiveness of the security controls. <br>

The metrics to be shown are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- NSG Inbound Malicious Flows Allowed 
<!-- -AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet) -->

### The architecture of the mini honeynet in Azure consists of the following:

- Virtual Network (VNet) *VLANs*
- Network Security Group (NSG) * Firewalls at Layer 3 and 4 *
- Virtual Machines (2 Windows, 1 Linux)
- Log Analytics Workspace
- Azure Key Vault *Enterprise level password manager*
- Azure Storage Account
- Microsoft Sentinel *SIEM*

For the "BEFORE" metrics, all resources (VM's) were originally deployed and intentionally exposed to the Internet.
The Virtual Machines had both their Network Security Groups and built-in firewalls wide open. All other resources were deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoints.

## Architecture Before Hardening / Security Controls

![Architecture Before Hardening _ Security Controls](https://github.com/esteban-flores/Azure-SOC-Honey-Net/assets/60724828/fc8bf287-b3d6-4ef9-a408-d76d3daba70a)

## Architecture After Hardening / Security Controls

![Architecture After Hardening _ Security Controls](https://github.com/esteban-flores/Azure-SOC-Honey-Net/assets/60724828/ad54d51b-f975-4331-98da-8132b0901d7b)

## Attack Maps Before Hardening / Security Controls

<h3>Windows RDP Authentication Failures</h3>

![before-windows-rdp-auth-fail](https://github.com/esteban-flores/Azure-SOC-Honey-Net/assets/60724828/6c0472c8-3ca1-4237-8091-a5bca2144cb1)<br>

<h3>Syslog SSh Authentication Failures</h3>

![before-syslog-linux-ssh-auth-fail](https://github.com/esteban-flores/Azure-SOC-Honey-Net/assets/60724828/123899df-b4a2-4cac-ae9c-d9c1ca3ada3a)<br>

<h3>MSSQL Authentication Failures</h3>

![Before-mssql-auth-fail](https://github.com/esteban-flores/Azure-SOC-Honey-Net/assets/60724828/52f1e407-a6d5-4d93-9edb-a0c3f528d735)<br>

<h3>NSG Malicious Allowed In</h3>

![before-nsg-malicious-allowed-in](https://github.com/esteban-flores/Azure-SOC-Honey-Net/assets/60724828/cf3ca6cc-6a6d-47fd-a862-126cd4fb25cf)<br>

<!--
  Josh's Maps
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/1qvswSX.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ESr9Dlv.png)<br>
-->

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-07-28 04:33:45
Stop Time 2023-07-29 04:33:45

| Metric                                                 | Count
| ------------------------------------------------------ | -----
| SecurityEvent (Windows VMS)                            | 27944
| Syslog (Linux VMs)                                     | 15009
| SecurityAlert (Microsoft Defender for Cloud)           | 12
| SecurityIncident (Sentinel Incidents)                  | 73
| NSG Inbound Malicious Flows Allowed                    | 1903

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-07-31 04:03:30
Stop Time	2023-08-01 04:03:30

| Metric                                                 | Count
| ------------------------------------------------------ | -----
| SecurityEvent (Windows VMS)                            | 27944
| Syslog (Linux VMs)                                     | 15009
| SecurityAlert (Microsoft Defender for Cloud)           | 12
| SecurityIncident (Sentinel Incidents)                  | 73
| NSG Inbound Malicious Flows Allowed                    | 1903

## Conclusion

A mini honeynet was created in Microsoft Azure. Log sources were integrated into a Log Analytics workspace. 
KQL queries were used to trigger alerts and create incidents in Microsoft Sentinel. 
Metrics were measured in an insecure environment before security controls were implemented. After implementing security measures, the metrics were done again to measure the difference. 
After the security controls were implemented, the number of security events and incidents were heavily reduced with certain types being reduced to zero.
The controls were effective but not given more time, it's certain that security events and alerts would begin to generate.
