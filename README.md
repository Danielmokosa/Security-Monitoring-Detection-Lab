# Splunk Active Directory Security Monitoring Lab

## Overview

This project demonstrates the deployment of a Splunk-based security monitoring environment within a Windows Active Directory domain.

A Splunk Universal Forwarder was configured on a Windows Server domain controller to securely forward Windows event logs into Splunk Cloud for centralized log analysis and security monitoring.

The environment was used to collect and analyze Windows Security, System, Application, and PowerShell logs, monitor Active Directory authentication activity, create detections, investigate security events, develop dashboards, and automate reporting workflows.

## Lab Architecture
![Lab Architecture](screenshots/Splunk-Active-Directory-Security-Monitoring-Lab.png)

### Technologies Used

#### Infrastructure

- Windows Server
- Active Directory Domain Services (AD DS)
- Domain Controller (MSIT-DC01)

#### SIEM & Monitoring

- Splunk Cloud
- Splunk Universal Forwarder
- Splunk Search Processing Language (SPL)

#### Log Sources

- Windows Security Logs
- Windows System Logs
- Windows Application Logs
- Windows PowerShell Logs

#### Security Use Cases

- Authentication Monitoring
- Failed Logon Detection
- Credential Validation Monitoring
- Privileged Group Change Detection
- Security Event Investigation
- Dashboard Development
- Detection Engineering

#### Reporting & Automation

- PowerShell
- CSV Reporting
- Automated Security Reporting

### Project Objectives

- Deploy Splunk Cloud and configure log forwarding
- Validate Windows event log ingestion
- Monitor Active Directory authentication events
- Create security detections and saved searches
- Build operational security dashboards
- Investigate security-relevant events
- Generate automated security reports

---
## Phase 1 — Environment Setup

Deployed a Splunk Cloud security monitoring environment and configured a Splunk Universal Forwarder on a Windows Server domain controller. The forwarder was connected to Splunk Cloud using the Splunk Cloud Universal Forwarder credentials package and verified through an active SSL forwarding connection on port 9997.

Windows Event Logs were successfully ingested into Splunk Cloud from the domain controller host `MSIT-DC01`, confirming that the log pipeline was operational.

### Screenshots

#### Universal Forwarder Connected to Splunk Cloud

![Universal Forwarder Connected](screenshots/proj2screenshots:phase1-forward-server.png)

#### Windows System Events Successfully Ingested

![System Events Part 1](screenshots/proj2screenshots:phase1-system-eventspt1.png)

![System Events Part 2](screenshots/proj2screenshots:phase1-system-eventspt2.png)

## Phase 2 — Log Ingestion Validation

Validated Windows event log ingestion into Splunk Cloud by confirming that events were being received from the domain controller and properly categorized by Splunk sourcetype. Searches were performed to verify active log collection and confirm host attribution for future monitoring and detection use cases.

### Evidence Captured

- Validated Windows event log ingestion into Splunk Cloud
- Verified event parsing through Splunk sourcetypes
- Confirmed host attribution for collected events
- Confirmed event collection from domain controller `MSIT-DC01`

### SPL Queries Used

#### Events by Sourcetype

```spl
index=* 
| stats count by sourcetype
```

#### Events by Host

```spl
index=* 
| stats count by host
```
  
### Events by Sourcetype

![Events by Sourcetype](screenshots/proj2screenshots:phase2-sourcetype-counts.png)

### Events by Host

![Events by Host](screenshots/proj2screenshots:phase2-host-counts.png)

## Phase 3 — Active Directory Authentication Monitoring

Monitored Active Directory authentication activity through Splunk Cloud by generating and analyzing successful logons, failed logons, and credential validation events. Authentication-related event IDs were investigated to validate security visibility and establish a foundation for detection engineering and incident response workflows.

### Evidence Captured

- Successful logon monitoring (Event ID 4624)
- Failed Authentication Activity (4776)
-  Credential Validation Activity
- Authentication event investigation through Splunk search
- Active Directory security event visibility validation

### SPL Queries Used

#### Successful Logons

```spl
EventCode=4624
```

#### Failed Authentication Activity

```spl
EventCode=4776
```

#### Credential Validation Activity Over Time

```spl
EventCode=4776
| timechart count
```

### Failed Authentication Activity (4776)

![Failed Authentication Activity](screenshots/proj2phase3-failed-authentication-4776.png)

### Successful Logon Activity (4624)

![Successful Logon Activity](screenshots/proj2phase3-successful-logon-4624.png)

### Credential Validation Activity
![Failed Authentication Activity](screenshots/proj2phase3-credentiall-validation-activity-4776.png)

## Phase 4 — Detection Engineering

Developed security detections within Splunk Cloud to identify authentication anomalies and privileged account activity within the Active Directory environment. Saved searches were created to monitor failed logons, Credential Validation Monitoring, and privileged group membership changes.

### Evidence Captured

- Created failed logon detection for authentication monitoring
- Created Credential Validation Monitoring detection for account abuse identification
- Created privileged group change detection for elevated access monitoring
- Established reusable detections for future alerting workflows

  ### SPL Queries Used

#### Successful Logon Detection

```spl
EventCode=4624
| stats count by Account_Name, host
```

#### Failed Authentication Detection

```spl
EventCode=4776
| stats count by Logon_Account, Source_Workstation, host
```

#### Privileged Group Change Detection

```spl
EventCode=4728 OR EventCode=4732 OR EventCode=4756
```

### Successful Logon Detection

![Successful Logon Detection](screenshots/proj2phase4-successful-logon-detection.png)
### Failed Authentication Detection

![Failed Authentication Detection](screenshots/proj2phase4-failed-authentication-detection.png)
### Privileged Group Change Detection

![Privileged Group Change Detection](screenshots/proj2phase4-privileged-group-detection.png)
## Phase 5 — Security Dashboard Development

Developed a centralized Active Directory security monitoring dashboard within Splunk Cloud to visualize authentication activity, failed logons, Credential Validation Monitoring
, and high-frequency Windows security events. Dashboard panels were created to support rapid investigation and security monitoring workflows.

### Evidence Captured

- Created centralized Active Directory monitoring dashboard
- Visualized successful authentication activity
- Visualized failed authentication activity
- Visualized Credential Validation Monitoring events
- Identified high-frequency Windows security event codes
- Established analyst-friendly monitoring views

### SPL Queries Used

#### Authentication Activity Panel

```spl
EventCode=4624
| timechart count
```

#### Failed Logons Panel

```spl
EventCode=4625
| timechart count
```

#### Credential Validation Activity Panel

```spl
EventCode=4776
| timechart count
```

#### Top Event Codes Panel

```spl
index=*
| top EventCode
```

### Active Directory Security Monitoring Dashboard

![Dashboard Overview](screenshots/proj2phase5-dashboard-overview.png)

### Authentication Activity Panel

![Authentication Activity](screenshots/proj2phase5-authentication-activity.png)

### Failed Logons Panel

![Failed Logons](screenshots/proj2phase5-failed-logons.png)

### Credential Validation Activity

![Credential Validation Activity](screenshots/proj2phase5-Credential-Validation-Activity.png)

### Top Event Codes Panel

![Top Event Codes](screenshots/proj2phase5-top-event-codes.png)

## Phase 6 — Automated Security Reporting

Developed a PowerShell-based reporting workflow to automate extraction of Windows security events for reporting and analysis.

Authentication-related events were exported from the Windows Security log into CSV format, demonstrating basic security reporting and automation capabilities.

### Evidence Captured

- PowerShell security reporting script execution
- Automated CSV report generation
- Export of Windows Security event data
- Basic reporting automation workflow

### PowerShell Script Used

```powershell
New-Item -ItemType Directory -Path C:\Reports -Force

Get-WinEvent -FilterHashtable @{
    LogName='Security'
    Id=4625
} |
Select-Object TimeCreated, Id, ProviderName, Message |
Export-Csv C:\Reports\FailedLogons.csv -NoTypeInformation
```

### PowerShell Report Generation

![PowerShell Report Generation](screenshots/proj2phase6-powershell-report-generation.png)

### Generated CSV Report

![Generated CSV Report](screenshots/proj2phase6-csv-created.png)

### CSV Report Contents

![CSV Report Contents](screenshots/proj2phase6-csv-results.png)

![CSV Report Contents Continued](screenshots/proj2phase6-csv-results2.png)

## Skills Demonstrated

- Active Directory Administration
- Windows Event Log Analysis
- Splunk Cloud Deployment
- Splunk Search Processing Language (SPL)
- Security Monitoring
- Authentication Investigation
- Detection Engineering
- Dashboard Development
- PowerShell Automation
- Security Reporting
