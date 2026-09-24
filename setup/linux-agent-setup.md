# Wazuh Agent Setup (Ubuntu-2)

- OS: Ubuntu Server (Ubuntu-2 VM), 2GB RAM, 2 cores
- Generated agent install command from Wazuh dashboard (Agents → Deploy new agent)
- Installed agent package, then enabled and started the service:
  sudo systemctl enable wazuh-agent
  sudo systemctl start wazuh-agent
- Verified agent shows as "Active" in dashboard under Endpoints
- Agent IP: 10.0.2.7, Agent name: ubuntu-2-agent-1