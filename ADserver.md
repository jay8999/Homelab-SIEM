# Active Directory Lab Setup Summary

**Author / Tutorial Reference:** Robert (*All Things IT*) — adapted from Josh Mator's Active Directory tutorial.  
**Objective:** Setting up a small local lab network with a Windows Server 2019 Domain Controller and a Windows 10 client, leading up to a Linux-based IT ticketing, SIEM, and IDS deployment.

---

## 1. Virtual Machine & Network Configuration

* **OS / Host Name:** Windows Server 2019 (`DC` / Domain Controller)
* **VM Resources:**
  * **Memory (RAM):** 10 GB
  * **Processors (vCPUs):** 4 cores
  * **Hard Drive Space:** 90 GB
* **Network Adapters (Dual-NIC Configuration):**
  * **Adapter 1 (NAT):** Provides external internet access (e.g., `10.0.2.15`).
  * **Adapter 2 (Internal Network):** Private network for internal client communication.
    * **Static IPv4 Address:** `172.16.0.1`
    * **Subnet Mask:** `255.255.255.0` (`/24`)
    * **Preferred DNS:** `127.0.0.1` (Loopback pointing to AD DS itself)

---

## 2. Active Directory Domain Setup

* **Guest Additions:** Installed virtualization guest tools and performed an initial system reboot.
* **Role Installation:** Added **Active Directory Domain Services (AD DS)** via *Add Roles and Features*.
* **Forest Promotion:** Promoted the server to a domain controller in a new forest:
  * **Domain Name:** `security.com`
  * **Directory Services Restoration Mode (DSRM) Password:** Set for emergency recovery.
* **Privileged Account Creation:**
  * Created a new Organizational Unit (OU) named **`admins`**.
  * Created a custom administrator account (`jay`) with a non-expiring password.
  * Added `jay` to the **Domain Admins** security group and verified login functionality.

---

## 3. Routing & DHCP Services

* **Remote Access & NAT:**
  * Installed the **Remote Access** role (specifically *Routing* and *Remote Access Service - RRAS*).
  * Configured NAT on RRAS, binding it to the internet adapter (`10.0.2.15`) so internal network clients can share internet access through the Domain Controller.
* **DHCP Server Role:**
  * Installed the **DHCP Server** role.
  * Created a new IPv4 Scope:
    * **Scope Name / Range:** `172.16.0.100` – `172.16.0.200`
    * **Subnet Mask:** `255.255.255.0` (`/24`)
    * **Gateway Option:** Configured with the DC's internal IP (`172.16.0.1`).
    * **Lease Duration:** Set to `99 days`.
    * **Activation:** Activated and verified scope status (green indicator).

---

## 4. Client Integration & Verification

* **Windows 10 Client Deployment:**
  * Created a Windows 10 VM configured on the same **Internal Network** adapter.
  * Verified automatic IP address assignment via DHCP (`172.16.0.100`) using `ipconfig`.
* **Domain Join:**
  * Renamed the PC to `Windows client` and joined it to `security.com` using the `jay` admin credentials.
  * Rebooted and logged in using a standard non-admin test user account.
* **Verification Console Checks:**
  * **AD Users and Computers:** Confirmed `Windows client` appeared under computer objects.
  * **DHCP Console:** Verified active IP lease for the client under *Address Leases*.

---

## 5. Next Steps: Preparing for the SIEM & Linux Server

With your Active Directory domain and Windows client operational, the next phase in your lab build is:
1. **Linux Server Setup:** Deploying a Linux server (Ubuntu/CentOS) on the network.
2. **Core Services:** Installing an IT Ticketing System, a SIEM platform (e.g., Elastic Stack / Wazuh / Graylog), and an Intrusion Detection System (IDS).
3. **Log Forwarding:** Configuring Windows Event Forwarding (WEF) or Syslog agents on the DC and Windows Client to pipe security logs into your new SIEM server.
"""

with open("active_directory_setup_summary.md", "w") as f:
    f.write(markdown_content)

print("Saved active_directory_setup_summary.md successfully.")
