# Azure Honeypot Deployment with Microsoft Sentinel SIEM Integration
This project documents my hands-on experience in building a cloud-based honeypot on Microsoft Azure, forwarding its logs to Microsoft Sentinel, and visualizing global attack sources through enriched data.

---

## 📘 Project Overview:

In an era where cyber threats are ever-evolving, the ability to monitor and analyze attack patterns in real-time is crucial. This project documents my hands-on experience in building a cloud-based honeypot on Microsoft Azure, forwarding its logs to Microsoft Sentinel, and visualizing global attack sources through enriched data. The goal was to simulate a vulnerable system, observe real-world attack attempts, and convert the collected data into actionable threat intelligence.

---

## 🎯 Project Objectives:
- Set up a Windows 10 honeypot in Microsoft Azure.
- Capture and send security event logs to Microsoft Sentinel.
- Enrich logs using a custom GeoIP database.
- Visualize attack origins with an interactive world map.
- Gain insights into common attacker behaviors and threat geographies.

---

## 🚀 My Approach and Execution:

### 1. Deploying the Honeypot in Azure
I began by logging into the Azure Portal and provisioning a new Windows 10 virtual machine. The configuration included enabling RDP access, setting a strong password, and exposing inbound ports by modifying the Network Security Group (NSG). This step effectively turned the VM into a lure for malicious actors scanning for open systems.

To maximize the honeypot’s visibility, I:
- Allowed all inbound traffic via NSG rule.
- Disabled the internal Windows Firewall using `wf.msc`.

### 2. Setting Up Log Collection via Microsoft Sentinel
Next, I created a Log Analytics Workspace and attached it to Microsoft Sentinel. I then connected the VM to the workspace using the "Windows Security Events via AMA" data connector. The setup involved:
- Creating a Data Collection Rule (DCR).
- Linking the VM and Log Analytics Workspace.

This ensured all login attempts, including failed attempts (Event ID 4625), were forwarded to Sentinel for analysis.

### 3. Raw Log Analysis
I queried the logs in Sentinel to view initial login attempts:

SecurityEvent
| where EventID == 4625
| project TimeGenerated, Account, IpAddress

### 4. Enriching Logs with GeoIP Data
To gain more context, I imported a massive GeoIP database (CSV format with ~54,000 rows) as a Sentinel Watchlist named geoip. This file contained IP ranges mapped to country, region, and city details.

### 5. Building the Attack Map
The final and most visually appealing part of the project was creating a custom Workbook in Microsoft Sentinel.

- I started a new workbook, removed default visuals, and added a new query control.

- Then, I imported a JSON template (map.json) that transformed the enriched data into an interactive world map.

- This visualization made it easy to detect high-volume attack regions at a glance.

### ✅ Outcome

- Successfully deployed a fully functioning honeypot on Azure.

- Connected logs from the honeypot in real-time to Microsoft Sentinel.

- Enriched raw IP logs with location data using a custom GeoIP watchlist.

- Created an interactive global attack map in Microsoft Sentinel, enabling visualization of attack origins.

### ✅ Learnings:

- Honeypot Deployment: Azure VM provides a scalable environment to simulate vulnerable systems. Proper NSG and firewall configurations allow for a controlled attack surface.

- Log Collection & Analysis: Integrating with Microsoft Sentinel allows for centralized log collection and analysis, essential for threat detection.

- Log Enrichment: Enriching logs with GeoIP data significantly improves the context and actionable insights derived from basic IP logs.

- Visualization: Building a custom attack map allows for real-time monitoring and understanding of global attack patterns, helping detect high-volume attack regions.

### 🔍 Key Insights:

- Attack Patterns: A significant portion of attacks originated from public IPs associated with cloud providers.

- Brute Force Attempts: Multiple brute force login attempts were detected within hours of deployment, indicating common attack behavior.

- Data Enrichment: Enriching raw telemetry with geographic data adds another layer of intelligence and makes threat data actionable.

- Cloud Vulnerabilities: The project highlighted the importance of securing cloud-hosted VMs against brute force attacks and misconfigurations.

### Conclusion:

This project has been an invaluable exercise in learning about real-world attack patterns, log collection, data enrichment, and visualization using Microsoft Sentinel. The combination of a honeypot, real-time monitoring, and enriched data helped create a robust threat detection and visualization system. The insights gained will be useful in enhancing cybersecurity posture and responding proactively to emerging threats.



