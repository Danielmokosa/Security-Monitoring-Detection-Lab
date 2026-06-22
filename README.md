# Security-Monitoring-Detection-Lab
 Deploy Splunk, ingest Windows logs, monitor Active Directory events, build dashboards, create detections, investigate incidents, and automate reports.

## Phase 1 — Environment Setup

Deployed a Splunk Cloud security monitoring environment and configured a Splunk Universal Forwarder on a Windows Server domain controller. The forwarder was connected to Splunk Cloud using the Splunk Cloud Universal Forwarder credentials package and verified through an active SSL forwarding connection on port 9997.

Windows Event Logs were successfully ingested into Splunk Cloud from the domain controller host `MSIT-DC01`, confirming that the log pipeline was operational.

**Evidence Captured:**
- Universal Forwarder active forwarding connection
- Windows System event logs received in Splunk Cloud
- Host, source, and sourcetype validation for `MSIT-DC01`

### Screenshots

#### Universal Forwarder Connected to Splunk Cloud

![Universal Forwarder Connected](screenshots/proj2screenshots:phase1-forward-server.png)

#### Windows System Events Successfully Ingested

![System Events Part 1](screenshots/proj2screenshots:phase1-system-eventspt1.png)

![System Events Part 2](screenshots/proj2screenshots:phase1-system-eventspt2.png)
