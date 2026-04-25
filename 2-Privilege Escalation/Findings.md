## Findings

During this investigation, Windows Security Event Logs were analysed within Splunk to identify signs of privilege escalation activity on the target system.

The following key events were identified:

- Event ID 4720: A new local user account (`attackerlab`) was created
- Event ID 4732: The user account was added to the local Administrators group
- Event ID 4672: A logon session with elevated privileges was detected

These events occurred in sequence within a short time window, indicating a deliberate attempt to escalate privileges after initial access.

The account `attackerlab` was consistently present across all related events, confirming it as the subject of the activity.

## Interpretation

This behaviour is consistent with post-compromise attacker activity, where a user account is created and added to privileged groups to maintain persistence and elevate access rights on the system.
