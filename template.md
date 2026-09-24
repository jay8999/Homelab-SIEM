# Playbook: [Alert Name / Threat Type]

## 📋 Metadata
* **Author:** [Your Name]
* **Target SIEM:** Wazuh
* **Associated Rule IDs:** [e.g., 5710, 5712]
* **Severity Level:** [Low / Medium / High / Critical]
* **Last Updated:** [Date]

---

## 1. Trigger Conditions
This playbook is initiated when Wazuh generates an alert indicating:
* Brief description of what triggers the alert (e.g., Multiple failed SSH login attempts from a single source IP within a short window).

---

## 2. Triage & Investigation Steps
Follow these steps in your Wazuh dashboard to determine if the alert is a True Positive:

1. **Check Alert Details:**
   * Navigate to Wazuh Dashboard > Modules > Security events.
   * Look at the `agent.name`, `rule.id`, and `full_log`.
2. **Contextualize the Source:**
   * Is the source IP internal or external? (Use GeoIP or check if it's your own management IP).
   * Is this a known service account or a user workstation?
3. **Check Frequency & Scope:**
   * Are there associated alerts (e.g., multiple targets being hit, or just one)?

---

## 3. Containment & Remediation
If verified as a **True Positive**, execute the following steps:

* **Isolate the Host (if applicable):** Disconnect the Wazuh agent or block network traffic.
* **Block the IP:** Update firewall rules or use Wazuh Active Response to automatically block the malicious IP.
* **Revoke Credentials:** If a user account was compromised, force a password reset.

---

## 📝 4. Documentation & Closure
* **False Positive Action:** If benign (e.g., a vulnerability scanner), tune the Wazuh decoder/rule or add an exception.
* **Ticket/Log:** Record the incident details, root cause, and actions taken in your homelab log.
