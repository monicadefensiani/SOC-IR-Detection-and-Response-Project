Incident Response & Detection Engineering Project (Azure SOC Lab)

Portfolio Project – SOC Analyst / Incident Responder / Threat Hunter
Technologies: Microsoft Sentinel, Microsoft Defender for Endpoint (MDE), Azure VM, Log Analytics, KQL, Honey Pot, Kali Linux
Focus: Detection Engineering, Incident Response, Threat Hunting, Adversary Simulation

📌 Project Overview

This project demonstrates a full end-to-end SOC workflow in Microsoft Azure, covering:

Security logging and telemetry ingestion

Detection engineering with Microsoft Sentinel (SIEM/SOAR)

Endpoint detection and response using Microsoft Defender for Endpoint (EDR)

Deployment of a Honey Pot to attract malicious activity

Brute-force attack simulation from Kali Linux (controlled lab environment)

Incident investigation, response, and threat hunting using KQL

This lab mirrors real-world SOC operations and highlights my hands-on capability in:

Alert triage

Incident investigation

MITRE ATT&CK mapping

Threat hunting

Containment and remediation

⚠️ All attacks are performed in an isolated Azure lab with explicit authorization.

🏗️ Architecture Diagram
┌────────────┐        ┌──────────────────┐
│ Kali Linux │ ───▶   │ Honeypot VM       │
│ (Attacker) │        │ (Exposed SSH/RDP)│
└────────────┘        └────────┬─────────┘
                               │
                               ▼
                    ┌────────────────────┐
                    │ Microsoft Defender │
                    │ for Endpoint (EDR) │
                    └────────┬───────────┘
                               │
                               ▼
                    ┌────────────────────┐
                    │ Microsoft Sentinel │
                    │ (SIEM + SOAR)      │
                    └────────────────────┘
🧰 Tools & Services Used

Microsoft Azure

Microsoft Sentinel

Microsoft Defender for Endpoint (MDE)

Azure Log Analytics Workspace

Azure Virtual Machines (Windows & Linux)

Kali Linux (Attacker)

Azure Network Security Groups (NSG)

Kusto Query Language (KQL)

🧪 Lab Environment Setup
Step 1: Create Azure Resource Group
az group create \
  --name SOC-Lab-RG \
  --location eastus

📸 Screenshot: Azure Resource Group Created

Step 2: Create Log Analytics Workspace
az monitor log-analytics workspace create \
  --resource-group SOC-Lab-RG \
  --workspace-name SOC-LAW

📸 Screenshot: Log Analytics Workspace Overview

Step 3: Enable Microsoft Sentinel

Navigate to Microsoft Sentinel in Azure Portal

Click Create

Select SOC-LAW

Enable Sentinel

📸 Screenshot: Sentinel Successfully Enabled

🖥️ Virtual Machines Deployment
1️⃣ Honeypot VM (Linux – SSH)

Ubuntu Server 22.04

Public IP enabled

SSH (Port 22) intentionally exposed

az vm create \
  --resource-group SOC-Lab-RG \
  --name Honeypot-VM \
  --image Ubuntu2204 \
  --admin-username honeypot \
  --generate-ssh-keys

📸 Screenshot: Honeypot VM Deployment

2️⃣ Windows Endpoint (MDE Protected)

Windows 10/11

Defender for Endpoint onboarded

📸 Screenshot: Windows VM Created

Step 4: Onboard VM to Microsoft Defender for Endpoint

Run onboarding script on Windows VM:

Set-MpPreference -DisableRealtimeMonitoring $false

📸 Screenshot: Device Appearing in MDE Portal

🍯 Honeypot Configuration
Enable Verbose SSH Logging
sudo nano /etc/ssh/sshd_config
LogLevel VERBOSE
sudo systemctl restart ssh

📸 Screenshot: SSH Logs Enabled

🧨 Attack Simulation (Controlled Lab)
Kali Linux Setup (Azure VM)
sudo apt update && sudo apt install hydra -y

📸 Screenshot: Kali Linux VM Ready

Brute Force Attack Simulation (SSH)
hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://<HONEYPOT_PUBLIC_IP>

✔️ Simulates credential access attempts
✔️ Generates real telemetry for detection

📸 Screenshot: Brute Force Attack in Progress

🚨 Detection & Alerting (Microsoft Sentinel)
Create Custom Analytics Rule (SSH Brute Force)
SecurityEvent
| where EventLevelName == "Failure"
| where AccountType == "User"
| summarize FailedAttempts=count() by IPAddress, Account
| where FailedAttempts > 10

📸 Screenshot: Analytics Rule Triggered

🔍 Incident Investigation
Sentinel Incident View

Source IP identified

Attack pattern confirmed

MITRE ATT&CK Mapping:

T1110 – Brute Force

📸 Screenshot: Incident Timeline

🧠 Threat Hunting Queries
Identify Repeated Authentication Failures
SecurityEvent
| where TimeGenerated > ago(24h)
| where EventID == 4625
| summarize count() by IPAddress, Account
Detect Lateral Movement Indicators
DeviceNetworkEvents
| where ActionType == "ConnectionAttempt"
| summarize count() by RemoteIP
🛡️ Response & Remediation
Automated Response (Logic App)

Block attacking IP

Disable targeted account

Notify SOC team

📸 Screenshot: Logic App Automation

📊 Outcome & Skills Demonstrated

✔ SIEM deployment and tuning
✔ EDR telemetry analysis
✔ Detection engineering with KQL
✔ Threat hunting methodologies
✔ Incident response lifecycle
✔ Azure security architecture

📎 Future Enhancements

MFA enforcement

UEBA integration

SOAR playbooks for ransomware

Advanced MITRE ATT&CK coverage

🧑‍💻 Author

[Monica Defensiani]
SOC Analyst | Incident Responder | Threat Hunter
