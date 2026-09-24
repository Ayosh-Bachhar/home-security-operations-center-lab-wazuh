# SSH Brute-Force Simulation

## Setup
- Attacker: Kali Linux VM (IP: 10.0.2.15)
- Target: Ubuntu-2 (IP: 10.0.2.7), user account "mark-ii"
- Confirmed network connectivity with `ping 10.0.2.7`

## Password List
Created a small custom wordlist (passlist.txt) with common weak passwords, 
including the actual account password to guarantee a successful hit.

## Attack Command
hydra -l mark-ii -P passlist.txt ssh://10.0.2.7

## Result
Hydra found a valid password on the first run, successfully logging in as 
mark-ii. This generated the exact pattern Wazuh is built to detect: 
multiple failed logins followed by a success.