# Triage-Playbook (Guide for handling Wazuh security alerts)

Trigger Conditions: The specific alert, rule, or threshold that initiates the playbook (e.g., multiple failed login attempts followed by a successful login).

Triage Steps (The Investigation): Instructions on how to gather context. This includes checking threat intelligence feeds, examining user behavior, and querying logs to determine if an alert is a true positive (actual threat) or a false positive.

Containment & Remediation: Actions to stop the threat if it's real, such as isolating a compromised host, disabling a compromised user account, or blocking a malicious IP address.

Escalation Criteria: Clear rules on when an incident is too severe for a Tier 1/Tier 2 analyst and needs to be handed over to senior incident responders or management.

Documentation & Closure: Guidelines on what details must be recorded in the ticketing system for auditing, metrics, and future post-mortems.
