# Configuring a SOC and Honeynet on Azure with Real-Time Traffic
![Honeynet / SOC in Azure](https://cdn.discordapp.com/attachments/1212167695055855697/1250095594370891939/Azure_Honey_Net.png?ex=6669b1bf&is=6668603f&hm=4ec1116b260f8e8c4d2c27370bdc784ad6779415c084f5510b64a28f74f69bfa&)
## Intro

In this project, I created a small honeynet within Azure, collecting logs from different resources into a Log Analytics workspace. This workspace is utilized by Microsoft Sentinel to generate attack maps, set off alerts, and document incidents. Initially, I monitored security metrics in an unprotected environment for a day. After implementing various security measures to reinforce the system, I conducted another 24-hour metric assessment. The findings are presented below, highlighting key security metrics:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Pre-Hardening Architecture / Security Controls
![Architecture Diagram](https://cdn.discordapp.com/attachments/1212167695055855697/1250097082178732176/Arch_before_hard.png?ex=6669b322&is=666861a2&hm=1b021bd7d428e34d656cac662f7bd3a6d6e6c2b044e7444186a39e6098ad7c94&)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://cdn.discordapp.com/attachments/1212167695055855697/1250097060175155282/Arch_After_Hard.png?ex=6669b31c&is=6668619c&hm=f6bc24f9952d01d235d5b86a319cc8c497ae9fd5fa74c931ebadf43d2dfc802b&)

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
![NSG Allowed Inbound Malicious Flows](https://cdn.discordapp.com/attachments/1212167695055855697/1250097015220862987/before_nsg-malicious-allowed-in.png?ex=6669b312&is=66686192&hm=eeee105f31e387406d7d74abeceb7f16ca6b645d79b2944dc3c855d9733de9a7&)<br>
![Linux Syslog Auth Failures](https://cdn.discordapp.com/attachments/1212167695055855697/1250096978252136559/before_linux-ssh-auth-fail.png?ex=6669b309&is=66686189&hm=e4f533790e582f2ba43acb72281343f41bf7e795de44303d52c2bbe0f2a8bc17&)<br>
![Windows RDP/SMB Auth Failures](https://cdn.discordapp.com/attachments/1212167695055855697/1250096991925702656/before_windows-rdp-auth-fail.png?ex=6669b30c&is=6668618c&hm=8dfa564ecbc3380abe4b278b939c69a609a0269ecf030c523b70b728e6a57944&)<br>
![SQL Server Auth Failures](https://cdn.discordapp.com/attachments/1212167695055855697/1250097003090673719/before_mssql-auth-fail.png?ex=6669b30f&is=6668618f&hm=ef239146b87dccb3c49b98b38c9d9f507cf7d965c7acd2724ab6a482d100849d&)<br>

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
![Metrics Change](https://cdn.discordapp.com/attachments/1212167695055855697/1250267752636223540/Untitled.png?ex=666a5215&is=66690095&hm=b9d4bcdbf35e7238940541caf5da96a733107c18b3c224dce4f78238e9be0a7c&)

## Incedent Response

In this incident response exercise, the machine was isolated from the network to prevent any potential spread of malware. After thorough investigation and analysis, it was confirmed that the suspicious activity was not due to real malware but was triggered by a test signature used to simulate an incident for training purposes. Had this been a real malware incident, the response would have included immediate escalation according to our standard operating procedures, involving key stakeholders and any other relevant teams. The steps taken in this exercise align with the NIST-800-61 guidelines, ensuring a systematic and effective response to security incidents. Below are the screenshots illustrating some of the steps of the incident response process.


![Detected](https://cdn.discordapp.com/attachments/1212167695055855697/1250182691027157032/image.png?ex=666a02dc&is=6668b15c&hm=1f21f914f41bb9ac348a3103a5a48b1049a036a84e8764e99843ce8c330b91d6&)
![More Info About Threat](https://cdn.discordapp.com/attachments/1212167695055855697/1250188390679187677/image.png?ex=666a082b&is=6668b6ab&hm=c2668f449c6c8f20e55b0ae4f7076f8c472399d0c576f7ad0b27129867a8bdf5&)
![Investigation](https://cdn.discordapp.com/attachments/1212167695055855697/1250173389512114360/image.png?ex=6669fa33&is=6668a8b3&hm=35bbab2b796c1b00b9367bc64efd0c1c7d3791ed5c34ce537410ebd65926ca4e&)
![Comments](https://cdn.discordapp.com/attachments/1212167695055855697/1250173306296995890/image.png?ex=6669fa1f&is=6668a89f&hm=667903d43929b78b73c7efe54836a0cb662921e894c7b467d92ba03ce7f99bcd&)
![Isolation Time](https://cdn.discordapp.com/attachments/1212167695055855697/1250178823883063436/image.png?ex=6669ff42&is=6668adc2&hm=6ed357033866bfe887e74955d08b41e6b1dde0c4269c4641cafc9e3778ce019a&)
![Isolation Cont](https://cdn.discordapp.com/attachments/1212167695055855697/1250181272534388889/image.png?ex=666a018a&is=6668b00a&hm=8d09735f4177743665cee4ca41395a974c65874dd8a38067308528c744205891&)
![Deleted Rule](https://cdn.discordapp.com/attachments/1212167695055855697/1250188933057482803/image.png?ex=666a08ad&is=6668b72d&hm=ea3d1fa45b399ff18b0c59544820cd98720a1d5b41a982b347202f31393b2b65&)


## Conclusion

In this project, a mini honeynet was set up in Microsoft Azure, with log sources integrated into a Log Analytics workspace and monitored using Microsoft Sentinel. The exercise followed NIST-800-53 and NIST-800-61 guidelines for regulatory compliance and incident response, respectively. Metrics were gathered before and after applying security controls, revealing significant improvements in security posture, with some metrics showing a complete reduction in incidents.The mock malware incident allowed for practical application of incident response procedures, demonstrating the importance of thorough investigation and immediate escalation. The effective analysis of NSG inbound connections further emphasized the value of robust network security configurations. This project highlights the critical need for comprehensive security measures and adherence to industry best practices in safeguarding cloud environments.
