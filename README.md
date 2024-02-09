# Azure Sentinel Guide: Mapping Cyber Attacks Worldwide

<a href="https://emmanueldalcanto.tech/azure-sentinel-guide" target="_blank">Azure Sentinel Guide: Mapping Cyber Attacks Worldwide</a>


## Summary

This repository contains scripts and instructions for setting up Azure Sentinel, a cloud-based Security Information and Event Management (SIEM) solution, in conjunction with a deliberately exposed virtual machine configured as a honeypot. The aim is to actively monitor and log cyber-attacks originating from various global IP addresses, with the resulting data visualized on a world map.

## Table of Contents

1. [Deploying VM for Global Attacks: Establishing a Cyber Honeypot](#deploying-vm-for-global-attacks-establishing-a-cyber-honeypot)
2. [Setting Up Log Analytics and VM Integration for Threat Detection](#setting-up-log-analytics-and-vm-integration-for-threat-detection)
3. [Azure Sentinel Configuration](#azure-sentinel-configuration)
4. [Disabling Windows Firewall for Enhanced Internet Accessibility](#disabling-windows-firewall-for-enhanced-internet-accessibility)
5. [Downloading and Configuring PowerShell Script for Geo Data Extraction](#downloading-and-configuring-powershell-script-for-geo-data-extraction)
6. [Executing Script for Geo Data Retrieval from Attackers](#executing-script-for-geo-data-retrieval-from-attackers)
7. [Creating Custom Log in Log Analytics Workspace](#creating-custom-log-in-log-analytics-workspace)
8. [Extracting Fields from Raw Custom Log Data](#extracting-fields-from-raw-custom-log-data)
9. [Final Stage: Mapping Geographic Data in Sentinel](#final-stage-mapping-geographic-data-in-sentinel)

---

## 1. Deploying VM for Global Attacks: Establishing a Cyber Honeypot

To begin, the process involves setting up a Virtual Machine (VM) intentionally exposed to the internet, inviting global attempts at infiltration once online. Subsequently, a honeypot is constructed to facilitate the implementation of countermeasures against potential threats. The Network Security group is then established, configuring inbound internet traffic to flow freely into the VM, a practice typically discouraged but essential for gaining insights into the diverse methods of cyber attacks, ranging from TCP pings to ICMP scans.

## 2. Setting Up Log Analytics and VM Integration for Threat Detection
Our focus is on building a robust infrastructure for log management and analysis. We start by creating a Log Analytics Workspace, the central hub for storing logs. This serves as the foundation for our SIEM tool, Azure Sentinel, allowing us to visualize geodata on a map for improved threat intelligence. We then configure Microsoft Defender for Cloud to gather VM logs and direct them to the Log Analytics Workspace, ensuring comprehensive data capture. Finally, we establish connectivity between the Workspace and our VM to optimize threat detection.

## 3. Azure Sentinel Configuration
In configuring Azure Sentinel, we streamline the setup of Microsoft Sentinel, our SIEM tool, to visualize attack data effectively. Beginning with Microsoft Sentinel creation, we integrate it into our workspace, consolidating log analytics for streamlined analysis. We focus on Event ID 4625, tracking failed login attempts, and leverage IP addresses for geolocation data to pinpoint attackers’ locations. This information is used to create a custom log, ingested into Azure’s Log Analytics Workspace and Sentinel. With Latitude, Longitude, and Country data, we accurately plot attackers on a map for enhanced threat detection.

## 4. Disabling Windows Firewall for Enhanced Internet Accessibility
In this step, we focus on turning off the Windows Firewall on our VM to facilitate faster internet discovery. By disabling the Firewall, we enable ICMP echo requests, essential for internet connectivity and discovery. After confirming the connectivity issue with a timed-out ping, we proceed to disable the Firewall settings, including Domain, Private, and Public Profiles. With echo requests now permitted, the ping successfully resumes, ensuring improved accessibility for internet-based activities.

## 5. Downloading and Configuring PowerShell Script for Geo Data Extraction
In this step, we download a crucial PowerShell Script from a trusted source, Josh Madakor, via GitHub. The script, “Custom_Security_Log_Exporter.ps1,” is essential for extracting geographic information from logs. Following the download, we open Windows PowerShell ISE on the VM, create a new script, and paste the downloaded code. The script is saved as “Log_Exporter” on the Desktop for easy access. Additionally, we obtain an API Key from Geolocation.io, ensuring access to geo data in the script. This key is inserted into the script to enable accurate geographic data extraction.

## 6. Executing Script for Geo Data Retrieval from Attackers
In this phase, we execute a critical script designed to retrieve geographic data from potential attackers. The script operates continuously in a loop, scanning the security log for failed login attempts. Upon detection of such events, it extracts the IP addresses associated with the failed logins and fetches their corresponding geographic data. Subsequently, the script generates a new log file containing this information. The output log file is stored at a specified path, facilitating easy access for further analysis.

## 7. Creating Custom Log in Log Analytics Workspace
This stage involves creating a custom log within the Log Analytics Workspace to facilitate the integration of specific log data, including geographic information, into our analytics platform. We begin by navigating back to the Log Analytics Workspace on Azure and selecting our designated Honeypot. From there, we proceed to create a custom log tailored to our requirements. The process involves copying the relevant logs from the VM and storing them in a new notepad file named “failed_rdp.log.” Subsequently, we specify the collection path for the log file and provide essential details such as the log name (“FAILED_RDP_WITH_GEO”).

## 8. Extracting Fields from Raw Custom Log Data
This step focuses on extracting essential fields from raw custom log data to enhance the analytical capabilities of our Log Analytics Workspace. Initially, we utilize specific code, which can be found on our GitHub repository, to extract all relevant data from the logs. This process enables the creation of distinct fields such as latitude, longitude, username, and country. As a result, when reviewing the log file within our VM, these extracted fields will be clearly delineated, facilitating easier interpretation and analysis.

## 9. Final Stage: Mapping Geographic Data in Sentinel
This last stage involves setting up a map visualization within Microsoft Sentinel to plot geographic data using latitude and longitude coordinates or country information. Initially, we create a workbook within Sentinel to organize and display our data effectively. Subsequently, we remove unnecessary widgets and execute a query by

```powershell
