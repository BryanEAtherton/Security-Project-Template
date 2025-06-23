# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

The goal of this project was to build and observe a honeynet in Azure that was left unsecured and open to the internet. Then secure the environment and observe the difference in activity. The environment consisted of 2 virtual machines and a few other resources. The activity of these resources was ingested into a Log Analytics workspace, and  Sentinel was then used to trigger alerts, create attack maps, and create incidents that I could respond to and resolve.  I recorded some security metrics of the insecure environment for 24 hours, applied security controls to harden the environment, and then recorded the same metrics after another 24 hours.  The metrics recorded are: 
- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The resources in the Azure Honeynet are:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (1 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

All the "BEFORE" metrics for the resources were originally deployed and exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls disabled, and all other resources were visible to the Internet with no Private endpoint.

![1  SC 7-Bondary protection before](https://github.com/user-attachments/assets/f5d96fb7-2f6b-4374-a623-7993ddeaeb94)



The "AFTER" metrics had hardened Network Security Groups by blocking all internet traffic except my local network, and all other resources were protected by their built-in firewalls as well as an established Private Endpoint.

![2 1 SC 7-Bondary protection after](https://github.com/user-attachments/assets/f1bc77e7-e23f-47ce-9a57-30fe8536dcfb)


# Attack Maps Before Hardening / Security Controls
<h2>Linux Virtual Machine<h2/>
  
![6  linux-ssh-auth-fail map before](https://github.com/user-attachments/assets/457e5371-71b2-4b61-ae79-abcf9c24544f)<br>
<h2>Network Security Group<h2/>
  
![7  nsg-malicious-allowed-in](https://github.com/user-attachments/assets/6d1a69fd-bfdd-4e1d-b2a6-ad1237b18142)<br>

<h2>Windows Virtual Machine<h2/>
  
![8  windows-rdp-auth-fail map before](https://github.com/user-attachments/assets/fa8b5e69-d96f-4831-a597-6b8f023326cf)<br>

## Metrics Before Hardening / Security Controls

<p>The following table shows the metrics we measured in our insecure environment for 24 hours:<p/>
  
Start Time 6/14/2025 8:20:56
  
Stop Time 6/15/2025 8:20:56

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 12710
| Syslog                   | 5899
| SecurityAlert            | 0
| SecurityIncident         | 148
| AzureNetworkAnalytics_CL | 1539

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

<p>The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:<p/>
  
Start Time 6/16/2025 8:14:43

Stop Time	6/17/2025 8:14:43

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 1923
| Syslog                   | 0
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

For this project, I created an environment in Azure vulnerable to penetration attempts on the internet. The activity was logged and ingested into a Log Analytics workspace, and Sentinel was utilized to create attack maps and incidents based on security alerts. The metrics of the alerts were measured before any security protocols were implemented, then again after hardening the environment. There was a significant decrease in security events, with most of the metrics measured reduced by 100% in the time recorded after implementing security protocols.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
