# Wazuh Manager Installation

- OS: Ubuntu Server (Ubuntu-1 VM), 4GB RAM, 4 cores
- Installed Wazuh 4.9.2 using the official all-in-one quickstart script:
  `curl -sO https://packages.wazuh.com/4.9/wazuh-install.sh && sudo bash ./wazuh-install.sh -a`
- Dashboard accessed via VirtualBox NAT Network port forwarding 
  (host port 8443 → guest 10.0.2.6:443)
- Logged into dashboard at https://localhost:8443 using generated admin credentials