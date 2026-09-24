# Home Security Operations Center Lab with Wazuh

## Objective
This project simulates a small home/enterprise Security Operations Center (SOC) 
using Wazuh, an open-source SIEM platform. It demonstrates the full detection 
lifecycle: building a monitored environment, generating a real attack, detecting 
it, investigating the alert, and documenting the incident — the core workflow 
of a SOC/Cyber Defense Analyst.

## Lab Architecture
- **Ubuntu-1 (Wazuh Manager)** — 4GB RAM, 4 cores. Runs Wazuh manager, indexer, 
  and dashboard. Receives and analyzes logs from agents.
- **Ubuntu-2 (Monitored Endpoint)** — 2GB RAM, 2 cores. Runs a Wazuh agent that 
  forwards system logs (including SSH authentication events) to the manager.
- **Kali Linux (Attacker)** — 4GB RAM, powered on only during the attack 
  simulation. Used to run a controlled SSH brute-force attack.
- All VMs connected via a VirtualBox NAT Network, isolated from the host's 
  main network.

Network layout:
- Ubuntu-1 (Manager): 10.0.2.6
- Ubuntu-2 (Agent): 10.0.2.7
- Kali (Attacker): 10.0.2.15

## Tools Used
- Wazuh 4.9.2 (Manager, Indexer, Dashboard)
- Ubuntu Server 
- Kali Linux
- Hydra (brute-force simulation tool)
- Oracle VirtualBox

## Setup
Detailed steps documented in:
- [Wazuh Manager Installation](setup/wazuh-manager-installation.md)
- [Wazuh Agent Setup](setup/linux-agent-setup.md)

## Attack Scenario
An SSH brute-force attack was launched from Kali against the Ubuntu-2 
endpoint's `mark-ii` account using Hydra with a custom password list. 
Full details: [SSH Brute-Force Simulation](attack-simulation/ssh-bruteforce-simulation.md)

## Detection
Wazuh detected the attack via its built-in rule for "Multiple authentication 
failures followed by a success" (Rule ID 40112, Severity: High). The alert 
was mapped to MITRE ATT&CK techniques T1110 (Brute Force) and T1078 
(Valid Accounts).

![Dashboard Overview](screenshots/dashboard.png)
![Agent Status](screenshots/agents.png)
![Alert Details](screenshots/alert-details.png)

## Incident Report
Full investigation and analysis: 
[Incident Report — SSH Brute-Force](reports/incident-report-ssh-bruteforce.md)

## What I Learned
- How a SIEM pipeline works end-to-end: log collection → decoding → rule 
  matching → alerting
- How to configure NAT networking between multiple VMs so they can 
  communicate while staying isolated from the host
- The difference between application-level credentials (e.g., a dashboard 
  login) and OS-level credentials — a mix-up I ran into and resolved during 
  this project
- How to read and interpret a real SIEM alert, including MITRE ATT&CK 
  mappings
- How to simulate and detect a real (if small-scale) brute-force attack 
  safely, inside an isolated lab

## Limitations
- Manager and agent were both lightweight VMs; a production SOC would run 
  these on dedicated, higher-spec hardware and monitor many more endpoints
- Only one attack scenario (SSH brute-force) was tested; a real SOC handles 
  many attack types
- Password list was small and custom-built for demonstration purposes, not 
  representative of a real-world attacker's wordlist

## Ethical Note
All attack simulation was performed exclusively against machines owned and 
controlled by the author, inside an isolated virtual network with no 
connection to production systems or third parties.