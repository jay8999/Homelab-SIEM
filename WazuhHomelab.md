# Building a Home SIEM Lab with Wazuh on VirtualBox
Project Overview

This is how I built a Security Information and Event Management (SIEM) environment using Wazuh.

What does it do?:

  Wazuh Server: Collects logs, hosts the web interface dashboard, and manages agents.

  Agent VMs (Linux & Windows): Generates telemetry, logs, and security events that stream back to the Wazuh server.

# Prerequisites & Lab Architecture

When setting up, I used my host machine which had 40GB of RAM. Ideally you would have 16GB+ RAM on your physical PC so you can spare resources for multiple VMs.

I created three virtual machines in VirtualBox:

  Wazuh Server VM (Ubuntu/Debian) — Recommended: 4 vCPUs, 8GB RAM, 50GB Storage.

  Linux Endpoint VM (Ubuntu) — To act as a monitored agent.

  Windows Endpoint VM (Windows 10/11) — To act as a monitored agent.

# Step 1: Setting Up the Wazuh Server

Deploy Ubuntu: Spinned up a fresh Ubuntu Virtual Machine in VirtualBox and made sure I could access the terminal.

Open Terminal & Elevate Privileges:

    sudo bash
    
Ran the Wazuh Quickstart Installation Script:

Wazuh provides an automated installation script that sets up the server, indexer, dashboard, and a local agent all at once using the -a flag.

    curl -sO https://packages.wazuh.com/4.10/wazuh-install.sh && sudo bash ./wazuh-install.sh -a
  
(Note: Ensure curl is installed on your VM beforehand).

Retrieved Credentials: Once the installation finished (usually takes around 5 minutes), the script will output a default username (admin) and a secure auto-generated password.

  1. Accessed the Web Interface by:
  
  2. Finding the IP address of my Wazuh server.
  
  3. Opened a browser and navigated to https://<WAZUH_SERVER_IP> (it runs on port 443 by default).
  
  4. Logged in with the admin credentials.

# Step 2: Deploying a Linux Agent

I then connected a secondary Linux machine as an agent.

  1. In the Wazuh dashboard homepage, I clicked on Deploy new agent.

  2. Select OS (Ubuntu in my case), entered my Wazuh server's IP address, and gave the agent a recognizable name (I chose Ubuntu).

  3. Copied the generated one line registration command found here:

         apt-get install wazuh-agent.

  5. Logged into the Ubuntu VM, opened a terminal and elevated to root (via sudo bash), and pasted the command.

  6. Started and enabled the Wazuh agent service using the commands provided in the dashboard:

    sudo systemctl daemon-reload
    sudo systemctl enable wazuh-agent
    sudo systemctl start wazuh-agent

  6. After the connection was verified, I returned back to the Wazuh dashboard. Within a few moments, the agent status was switched to Active. Afterwards, I explored the modules for vulnerability detection and MITRE ATT&CK alerts.

# Step 3: Deploying a Windows Agent

Next, it was time to monitor a Windows host to capture Windows Event Logs, registry modifications, and file integrity monitoring (FIM).

  1. In the Wazuh dashboard, I clicked Deploy new agent and selected Windows.

  2. I set the agent name as WindowsClient and copied the provided PowerShell command.

  3. Opened an Administrator PowerShell prompt on the Windows VM and pasted the command to download and registered the agent.

  4. Ran this command to start the Wazuh service on Windows:

    Start-Service wazuh-ssec

  5. The dashboard confirmed the Windows agent connected successfully.

# Step 4: Generating Security Noise & Testing

To verify that the SIEM is successfully collecting telemetry and firing alerts, I generated some activity on each of the endpoints:

  On Linux: Elevated to root (sudo bash), ran commands like whoami, or install/remove packages. Checked the Wazuh dashboard under Threat Hunting / Events to see the captured privilege escalations and sudo logs.

  On Windows: Installed new software (like Google Chrome). Looked for File Integrity Monitoring (FIM) alerts highlighting registry changes and software installation logs in the Wazuh UI.

# Customization

  Enable Dark Mode: I prefered dark mode over the default theme, if you wanted to as well, go to Settings > Dashboard Management > Advanced Settings, scroll down to Appearance, toggle dark mode on, and click Save Changes.

  
