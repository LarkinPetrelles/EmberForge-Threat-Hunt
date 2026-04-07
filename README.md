# EmberForge Threat Hunt

Simulated threat hunt using Microsoft Sentinel focused on identifying an Active Directory compromise, credential access, lateral movement, and data exfiltration.

---

## What happened (quick summary)

- Initial access via malicious download using certutil  
- Payload execution and persistence established  
- Credentials dumped from LSASS  
- Lateral movement across domain systems  
- Data staged and exfiltrated using rclone to MEGA  

This simulates a full Active Directory compromise from initial access to exfiltration.

---

## Attack Overview

High level view of the investigation scope and activity timeline:

![Attack Overview](images/TH01_overview_attack_scope.jpeg)

---

## Initial Access + Execution

Payload retrieved and executed using built in Windows utilities and suspicious process chains.

Certutil used to download payload:

![Certutil Download](images/TH01_initial_certutil_download.jpeg)

Payload execution observed:

![Payload Execution](images/TH01_initial_payload_execution.jpeg)

Suspicious rundll32 execution chain:

![Rundll32 Execution](images/TH01_exec_rundll32_chain.jpeg)

Process tree showing abnormal behavior:

![Process Tree](images/TH01_exec_suspicious_process_tree.jpeg)

---

## Credential Access

Credential dumping activity identified through LSASS access and memory artifacts.

LSASS dump activity:

![LSASS Dump](images/TH01_cred_lsass_dump.jpeg)

Sensitive process access tied to credential theft:

![Sensitive Process Access](images/TH01_cred_sensitive_process_access.jpeg)

---

## Lateral Movement

Authentication and system access expanded across the environment.

Net use authentication activity:

![Net Use Auth](images/TH01_lat_net_use_auth.jpeg)

Remote file copy between systems:

![Remote Copy](images/TH01_lat_remote_copy.jpeg)

Shared resource access:

![Share Access](images/TH01_lat_share_access.jpeg)

---

## Staging + Exfiltration

Data prepared locally and exfiltrated using external tooling.

Data staging and preparation:

![Data Prep](images/TH01_stage_data_preparation.jpeg)

Archive creation prior to exfiltration:

![Archive Creation](images/TH01_stage_archive_creation.jpeg)

Rclone execution for data transfer:

![Rclone Execution](images/TH01_exfil_rclone_execution.jpeg)

Connection to MEGA infrastructure:

![Mega Connection](images/TH01_exfil_mega_connection.jpeg)

---

## Persistence

Mechanisms established to maintain access.

Account creation for persistence:

![Account Creation](images/TH01_persist_account_creation.jpeg)

Remote access tooling deployed:

![Remote Access Tool](images/TH01_persist_remote_access_tool.jpeg)

---

## Key Findings

- Domain compromise achieved  
- Credentials successfully extracted from LSASS  
- Multiple systems accessed through lateral movement  
- Data exfiltrated to external infrastructure (MEGA)  
- Persistence established via account creation and remote tooling  

Attack chain reflects common real world adversary behavior across enterprise environments.

---

## Tools + Skills Used

- Microsoft Sentinel (KQL)  
- Log analysis and correlation  
- Threat hunting methodology  
- Understanding of attacker TTPs (execution, credential access, lateral movement, exfiltration)  
