# Configuring a SOC and Honeynet on Azure with Real-Time Traffic
![Honeynet / SOC in Azure](https://github.com/TheAfricanAfro/Honeynet-Azure/blob/63b44db4caf3c81f3dc6ebcabd1062fd83b45756/Images/Azure%20Honey%20Net.png)
## Intro

In this project, I created a small honeynet within Azure, collecting logs from different resources into a Log Analytics workspace. This workspace is utilized by Microsoft Sentinel to generate attack maps, set off alerts, and document incidents. Initially, I monitored security metrics in an unprotected environment for a day. After implementing various security measures to reinforce the system, I conducted another 24-hour metric assessment. The findings are presented below, highlighting key security metrics:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Pre-Hardening Architecture / Security Controls
![Architecture Diagram](https://github.com/TheAfricanAfro/Honeynet-Azure/blob/44ab6605a609e00612876258e44cf10579149a16/Images/Arch%20before%20hard.png)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://github.com/TheAfricanAfro/Honeynet-Azure/blob/a342b09970463a0907af0dd1a23c167815891be2/Images/Arch%20After%20Hard.png)

The architecture of the mini honeynet in Azure consists of the following components:

- Microsoft Sentinel
- Log Analytics Workspace
- Azure Storage Account
- Azure Key Vault
- Virtual Network (VNet)
- Network Security Groups (NSGs)
- Virtual Machines
  <ul>
  <li>Attacker Machine(Windows 10)</li>
  <li>Vuln Windows Machine (Windows 10)</li>
  <li>Vuln Linux Machine (Ubuntu Server)</li>
   </ul>


For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet.

For the "AFTER" metrics, the Network Security Groups were hardened by blocking all traffic except from my workstation. Additionally, all other resources were protected using their built-in firewalls.

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://github.com/TheAfricanAfro/Honeynet-Azure/blob/a342b09970463a0907af0dd1a23c167815891be2/Images/(before)%20nsg-malicious-allowed-in.png)<br>
![Linux Syslog Auth Failures](https://github.com/TheAfricanAfro/Honeynet-Azure/blob/a342b09970463a0907af0dd1a23c167815891be2/Images/(before)%20linux-ssh-auth-fail.png)<br>
![Windows RDP/SMB Auth Failures](https://github.com/TheAfricanAfro/Honeynet-Azure/blob/a342b09970463a0907af0dd1a23c167815891be2/Images/(before)%20windows-rdp-auth-fail.png)<br>
![SQL Server Auth Failures](https://github.com/TheAfricanAfro/Honeynet-Azure/blob/a342b09970463a0907af0dd1a23c167815891be2/Images/(before)%20mssql-auth-fail.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-06-05 22:26
Stop Time 2024-06-06 22:26

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 25593
| Syslog                   | 24582
| SecurityAlert            | 2
| SecurityIncident         | 356
| AzureNetworkAnalytics_CL | 2515

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-06-08 08:32
Stop Time	2024-06-09 08:32

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 11084
| Syslog                   | 2
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Change in Metrics
![Metrics Change](https://github.com/TheAfricanAfro/Honeynet-Azure/blob/fac39f87a4bf685b957602fc0ad25e06dc8ca7ae/Images/Metric%20Change.png)

## Incedent Response

In this incident response exercise, the machine was isolated from the network to prevent any potential spread of malware. After thorough investigation and analysis, it was confirmed that the suspicious activity was not due to real malware but was triggered by a test signature used to simulate an incident for training purposes. Had this been a real malware incident, the response would have included immediate escalation according to our standard operating procedures, involving key stakeholders and any other relevant teams. The steps taken in this exercise align with the NIST-800-61 guidelines, ensuring a systematic and effective response to security incidents. Below are the screenshots illustrating some of the steps of the incident response process.


![Detected](https://github.com/TheAfricanAfro/Honeynet-Azure/blob/4e6b702a0268961a5fd84d0c9a973de2f289b9f4/Images/Malware%20Detected.png)
![More Info About Threat](https://github.com/TheAfricanAfro/Honeynet-Azure/blob/c97c7f08e25ddcc1c00849244f0f659acac9d777/Images/Investigat%20event.png)
![Investigation](https://github.com/TheAfricanAfro/Honeynet-Azure/blob/c97c7f08e25ddcc1c00849244f0f659acac9d777/Images/Invest%20cont.png)
![Comments](https://github.com/TheAfricanAfro/Honeynet-Azure/blob/4e6b702a0268961a5fd84d0c9a973de2f289b9f4/Images/Comments.png)
![Isolation Time](https://github.com/TheAfricanAfro/Honeynet-Azure/blob/a950edce10fbafdbef2e5e26e7c758c207e6618f/Images/machine%20stopped.png)
![Isolation Cont](https://github.com/TheAfricanAfro/Honeynet-Azure/blob/5ee3738b5670a441ca02cbba62343d5e82737de7/Images/Added%20new%20Rule.png)
![Deleted Rule](https://github.com/TheAfricanAfro/Honeynet-Azure/blob/5ee3738b5670a441ca02cbba62343d5e82737de7/Images/Deleted%20Rule.png)


## Conclusion

In this project, a mini honeynet was set up in Microsoft Azure, with log sources integrated into a Log Analytics workspace and monitored using Microsoft Sentinel. The exercise followed NIST-800-53 and NIST-800-61 guidelines for regulatory compliance and incident response, respectively. Metrics were gathered before and after applying security controls, revealing significant improvements in security posture, with some metrics showing a complete reduction in incidents.The mock malware incident allowed for practical application of incident response procedures, demonstrating the importance of thorough investigation and immediate escalation. The effective analysis of NSG inbound connections further emphasized the value of robust network security configurations. This project highlights the critical need for comprehensive security measures and adherence to industry best practices in safeguarding cloud environments.
