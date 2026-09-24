# Incident Report: SSH Brute-Force Login (Successful)

## Incident Summary
A brute-force SSH login attack was conducted against the Ubuntu-2 endpoint 
(agent: ubuntu-2-agent-1) targeting user account "mark-ii". Multiple failed 
login attempts were followed by a successful authentication, indicating 
credential compromise.

## Timeline
- 02:32:22 (system time) — SSH login attempts begin against mark-ii from 10.0.2.15
- 02:32:24 — Wazuh generates alert (rule 40112): "Multiple authentication 
  failures followed by a success"

## Affected Host
- Agent: ubuntu-2-agent-1
- Agent IP: 10.0.2.7
- Targeted account: mark-ii

## Source of Attack
- Source IP: 10.0.2.15 (Kali Linux attacker VM)
- Source port: 52896
- Method: SSH password brute-force via Hydra

## Evidence
- Rule ID: 40112, Level: 12 (High)
- Rule description: "Multiple authentication failures followed by a success"
- Raw log: `Sep 24 20:32:22 mark-ii-VirtualBox sshd-session[17681]: Accepted 
  password for mark-ii from 10.0.2.15 port 52896 ssh2`
- MITRE ATT&CK mapping: T1110 (Brute Force), T1078 (Valid Accounts)
- MITRE tactics: Initial Access, Credential Access, Defense Evasion, 
  Persistence, Privilege Escalation

## Analysis
The attacker used a dictionary-based password guessing tool (Hydra) with a 
list of common/weak passwords. After several failed attempts, one password 
matched the account's actual credential, resulting in successful 
authentication. Wazuh correctly detected this pattern using its built-in 
sshd brute-force detection rule.

## Containment
In this lab, the attack was allowed to complete to demonstrate detection. 
In a real environment, immediate containment would include:
- Disabling or resetting the compromised "mark-ii" account
- Blocking the source IP (10.0.2.15) at the firewall
- Reviewing whether the attacker accessed anything after login
- Rotating credentials for any other accounts using similar weak passwords

## Recommendation
- Enforce strong password policy for all accounts
- Enable SSH key-based authentication and disable password login
- Implement fail2ban or similar to auto-block repeated failed login sources
- Enable multi-factor authentication where possible

## Lessons Learned
This exercise confirmed that weak passwords remain a critical attack vector, 
and that SIEM tools like Wazuh can reliably detect brute-force patterns 
using log analysis and correlation rules.