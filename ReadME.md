# EmberForge Threat Hunt

Simulated threat hunt using Microsoft Sentinel focused on identifying an Active Directory compromise, credential access, lateral movement, and data exfiltration.

---

## Attack Overview

![Attack Overview](images/TH01_overview_attack_scope.jpeg)

---

## Initial Access + Execution

- Payload downloaded using certutil
- Execution via suspicious binaries and rundll32

![Certutil Download](images/TH01_initial_certutil_download.jpeg)
![Payload Execution](images/TH01_initial_payload_execution.jpeg)
![Rundll32 Execution](images/TH01_exec_rundll32_chain.jpeg)
![Process Tree](images/TH01_exec_suspicious_process_tree.jpeg)

---

## Credential Access

- LSASS dump activity observed
- Sensitive process access confirmed

![LSASS Dump](images/TH01_cred_lsass_dump.jpeg)
![Sensitive Process Access](images/TH01_cred_sensitive_process_access.jpeg)

---

## Lateral Movement

- net use authentication
- Remote file copy
- Share access activity

![Net Use Auth](images/TH01_lat_net_use_auth.jpeg)
![Remote Copy](images/TH01_lat_remote_copy.jpeg)
![Share Access](images/TH01_lat_share_access.jpeg)

---

## Staging + Exfiltration

- Data staged locally
- Exfiltration via rclone to MEGA

![Data Prep](images/TH01_stage_data_preparation.jpeg)
![Archive Creation](images/TH01_stage_archive_creation.jpeg)
![Rclone Execution](images/TH01_exfil_rclone_execution.jpeg)
![Mega Connection](images/TH01_exfil_mega_connection.jpeg)

---

## Persistence

- Account creation
- Remote access tooling established

![Account Creation](images/TH01_persist_account_creation.jpeg)
![Remote Access Tool](images/TH01_persist_remote_access_tool.jpeg)

---

## Summary

End to end attack chain identified:
Initial access → execution → credential theft → lateral movement → staging → exfiltration → persistence

Focused on real attacker behavior using KQL and log analysis rather than alerts alone.