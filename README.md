# EmberForge Threat Hunt

Simulated threat hunt using Microsoft Sentinel focused on identifying an Active Directory compromise, credential access, lateral movement, and data exfiltration.

## What happened

- Initial access through a malicious download using certutil
- Payload execution and persistence established
- Credentials dumped from LSASS
- Lateral movement across internal systems
- Data staged and exfiltrated with rclone to MEGA

This threat hunt followed the attack from initial execution through credential theft, lateral movement, persistence, and exfiltration.

## Attack Overview

![Attack Overview](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_overview_attack_scope.jpeg)

## Initial Access and Execution

Certutil download activity:

![Certutil Download](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_initial_certutil_download.jpeg)

Initial payload execution:

![Initial Payload Execution](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_initial_payload_execution.jpeg)

Suspicious rundll32 execution chain:

![Rundll32 Chain](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_exec_rundll32_chain.jpeg)

Suspicious process tree tied to execution:

![Suspicious Process Tree](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_exec_suspicious_process_tree.jpeg)

## Credential Access

LSASS dump evidence:

![LSASS Dump](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_cred_lsass_dump.jpeg)

Sensitive process access tied to credential theft:

![Sensitive Process Access](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_cred_sensitive_process_access.jpeg)

## Lateral Movement

Net use authentication activity:

![Net Use Auth](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_lat_net_use_auth.jpeg)

Remote copy between hosts:

![Remote Copy](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_lat_remote_copy.jpeg)

Shared access created for staging and movement:

![Share Access](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_lat_share_access.jpeg)

## Staging and Exfiltration

Data preparation before transfer:

![Stage Data Preparation](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_stage_data_preparation.jpeg)

Archive creation before exfiltration:

![Stage Archive Creation](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_stage_archive_creation.jpeg)

Rclone execution:

![Rclone Execution](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_exfil_rclone_execution.jpeg)

External MEGA connection evidence:

![MEGA Connection](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_exfil_mega_connection.jpeg)

## Persistence

Backdoor account creation:

![Account Creation](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_persist_account_creation.jpeg)

Remote access tool persistence:

![Remote Access Tool](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_persist_remote_access_tool.jpeg)

## Key Takeaways

- Confirmed credential dumping from LSASS
- Confirmed lateral movement across internal systems
- Confirmed data staging and exfiltration to an external MEGA destination
- Confirmed persistence through account creation and remote access tooling

## Tools Used

- Microsoft Sentinel
- KQL
- Threat hunting
- Process analysis
- Credential access investigation
- Lateral movement analysis
- Exfiltration tracking
