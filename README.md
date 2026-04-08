# EmberForge Threat Hunt

Threat hunting investigation conducted in Microsoft Sentinel to identify and trace a full Active Directory compromise involving credential access, lateral movement, persistence, and data exfiltration.

---

## Experience Context

Log(n) Pacific LLC  
Role: Cyber Security Support Analyst (Vulnerability Management & SecOps Intern)

This investigation reflects hands on experience applying threat detection and analysis techniques in a SIEM environment. While the scenario is simulated, the workflow mirrors real SecOps responsibilities including log analysis, attack chain reconstruction, and identifying attacker behavior across multiple systems.

The focus was on identifying credential access, tracking lateral movement, detecting data exfiltration, and understanding how attackers maintain persistence within an environment.

This work aligns with practical security operations tasks such as investigating suspicious activity, correlating events across hosts, and mapping behavior to real world attack patterns.

---

## Executive Summary

Investigation identified a multi stage attack chain leveraging native Windows utilities and commodity tooling to achieve domain level access and exfiltrate internal data.

Key activity included:

- Initial payload delivery using certutil (living off the land)
- Execution via suspicious process chains including rundll32
- Credential dumping through LSASS memory access
- Lateral movement using native Windows authentication and shares
- Data staging and compression prior to exfiltration
- Exfiltration to external infrastructure using rclone (MEGA)
- Persistence established through account creation and remote access tooling

This behavior aligns closely with real world post exploitation and data theft activity observed in enterprise environments.

---

## Attack Timeline and Scope

High level view of activity across the environment:

![Attack Overview](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_overview_attack_scope.jpeg)

---

## Initial Access and Execution

Certutil was used to retrieve a payload from an external source. This is a common technique used to bypass traditional detection by leveraging built in tools.

![Certutil Download](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_initial_certutil_download.jpeg)

Payload execution followed immediately after download, indicating successful initial access.

![Initial Payload Execution](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_initial_payload_execution.jpeg)

Execution chain included rundll32, a known technique for proxy execution and defense evasion.

![Rundll32 Chain](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_exec_rundll32_chain.jpeg)

Process tree analysis shows abnormal parent child relationships consistent with malicious execution.

![Suspicious Process Tree](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_exec_suspicious_process_tree.jpeg)

---

## Credential Access

LSASS memory access was observed, confirming credential dumping activity.

![LSASS Dump](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_cred_lsass_dump.jpeg)

Additional telemetry shows sensitive process access tied directly to credential extraction.

![Sensitive Process Access](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_cred_sensitive_process_access.jpeg)

At this stage, the attacker likely obtained reusable credentials enabling further access across the environment.

---

## Lateral Movement

Authentication activity using net use indicates movement between systems using valid credentials.

![Net Use Auth](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_lat_net_use_auth.jpeg)

Remote file copy activity confirms transfer of tooling or payloads between hosts.

![Remote Copy](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_lat_remote_copy.jpeg)

Shared resource access suggests use of administrative shares to expand access.

![Share Access](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_lat_share_access.jpeg)

This pattern is consistent with post compromise lateral movement using native Windows capabilities.

---

## Staging and Exfiltration

Data was prepared locally prior to exfiltration, indicating intent to extract valuable information.

![Stage Data Preparation](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_stage_data_preparation.jpeg)

Archive creation activity shows compression of data before transfer.

![Stage Archive Creation](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_stage_archive_creation.jpeg)

Rclone was used to transfer data externally, a known tool for stealthy exfiltration.

![Rclone Execution](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_exfil_rclone_execution.jpeg)

Outbound connection to MEGA confirms successful exfiltration to external infrastructure.

![MEGA Connection](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_exfil_mega_connection.jpeg)

---

## Persistence

A new account was created, providing continued access even if initial entry points were removed.

![Account Creation](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_persist_account_creation.jpeg)

Remote access tooling was also deployed, further reinforcing persistence.

![Remote Access Tool](https://raw.githubusercontent.com/LarkinPetrelles/EmberForge-Threat-Hunt/main/TH01_persist_remote_access_tool.jpeg)

---

## Impact Assessment

- Compromise of domain credentials via LSASS dumping  
- Unauthorized access across multiple systems  
- Internal data staged and exfiltrated externally  
- Persistence mechanisms established to maintain access  

If this occurred in a production environment, it would represent a full domain compromise with high risk of data loss and continued attacker presence.

---

## Key Findings

- Credential dumping confirmed through LSASS access  
- Lateral movement performed using legitimate Windows tools  
- Data staged, compressed, and exfiltrated using rclone  
- External exfiltration destination identified (MEGA)  
- Persistence established via account creation and remote access  

Attack behavior closely matches known adversary techniques used in enterprise breaches.

---

## Tools and Techniques

- Microsoft Sentinel  
- KQL (Kusto Query Language)  
- Threat hunting methodology  
- Process and command line analysis  
- Credential access detection  
- Lateral movement tracking  
- Data exfiltration analysis

---

## Detection Opportunities

- Monitor for certutil usage with external downloads  
- Alert on LSASS access from non system processes  
- Detect abnormal use of net use and admin shares  
- Monitor for rclone or similar tools in command line activity  
- Track outbound connections to known file sharing services  

## Response Considerations

- Isolate affected systems  
- Reset compromised credentials  
- Investigate scope of lateral movement  
- Block external exfiltration endpoints  
- Remove persistence mechanisms
