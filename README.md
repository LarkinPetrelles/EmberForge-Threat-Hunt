# EmberForge Threat Hunt: Active Directory Attack Investigation

This repository is a breakdown of a threat hunt I completed in Microsoft Sentinel for the **EmberForge: Source Leak** investigation. The scenario followed an attacker moving through an environment, stealing credentials, moving laterally, staging data, exfiltrating files, and establishing persistence.

I worked through all 10 phases of the hunt and solved all 44 flags. The goal of this repo is to show how I used KQL, process telemetry, event logs, and host level evidence to piece the attack together. 

## Environment

**Platform used:** Microsoft Sentinel  
**Primary log source:** `EmberForgeX_CL`  
**Investigation focus:** Active Directory attack investigation  
**Host scope:**
- EC2AMAZ-B9GHHO6 → workstation
- EC2AMAZ-16V3AU4 → server
- EC2AMAZ-EEU3IA2 → domain controller

## What I investigated

During the hunt, I traced activity across the environment and identified evidence of:

- initial payload download and execution
- rundll32 based execution
- suspicious process chains
- LSASS dumping and sensitive process access
- user enumeration and credential use
- SMB share access and remote file copy
- net use authentication activity
- archive creation and data staging
- rclone usage and Mega exfiltration
- account creation for persistence
- remote access tool installation
- shadow copy deletion and anti forensic behavior

## Main skills used

- KQL hunting in Microsoft Sentinel
- Sysmon event analysis
- Windows event log investigation
- process tree analysis
- credential access detection
- lateral movement tracking
- persistence identification
- attack chain reconstruction

## Investigation flow

### 1. Initial access and execution

I started by identifying how the malicious payload got onto the host and what executed first. From there, I pivoted into suspicious parent child relationships and process chains to confirm the attacker execution path.

**Screenshots**
- `TH01_initial_certutil_download.jpeg`
- `TH01_initial_payload_execution.jpeg`
- `TH01_exec_rundll32_chain.jpeg`
- `TH01_exec_suspicious_process_tree.jpeg`

**What this showed**
- certutil was used to pull down a payload
- the payload was then executed
- rundll32 was involved in execution activity that stood out from normal system behavior

---

### 2. Credential access

After confirming execution, I looked for signs of credential theft. That led to LSASS related evidence and suspicious access to sensitive processes.

**Screenshots**
- `TH01_cred_lsass_dump.jpeg`
- `TH01_cred_sensitive_process_access.jpeg`

**What this showed**
- LSASS memory dumping activity occurred
- the attacker accessed a high value process to obtain credentials

---

### 3. Discovery and account targeting

The next part of the hunt was figuring out what the attacker was learning about the environment and what accounts they were working with.

**Screenshots**
- `TH01_discovery_user_enum.jpeg`
- `TH01_persist_account_creation.jpeg`

**What this showed**
- account enumeration activity using built in commands
- later on, a new account was created for persistence

---

### 4. Lateral movement

Once credentials were available, I traced how the attacker moved from one host to another. The evidence here came from authentication commands, share access, and remote file copy activity.

**Screenshots**
- `TH01_lat_net_use_auth.jpeg`
- `TH01_lat_share_access.jpeg`
- `TH01_lat_remote_copy.jpeg`

**What this showed**
- `net use` activity tied to remote access
- SMB share interaction between systems
- remote file transfer used to push tooling or payloads

---

### 5. Staging and exfiltration

After lateral movement, I moved into the data theft side of the attack. I looked for compression activity, staging behavior, and outbound transfer tooling.

**Screenshots**
- `TH01_stage_archive_creation.jpeg`
- `TH01_stage_data_preparation.jpeg`
- `TH01_exfil_rclone_execution.jpeg`
- `TH01_exfil_mega_connection.jpeg`

**What this showed**
- files were compressed and staged before transfer
- rclone was used as the exfiltration tool
- outbound activity connected to Mega infrastructure

---

### 6. Persistence and remote access

The attacker did not just steal data and leave. I also found evidence that they set themselves up to come back later.

**Screenshots**
- `TH01_persist_account_creation.jpeg`
- `TH01_persist_remote_access_tool.jpeg`

**What this showed**
- a new account was created in the environment
- a remote access tool was installed for continued access

---

### 7. Defense evasion

To round out the attack chain, I looked for anything that showed an attempt to reduce visibility or make investigation harder.

**Screenshots**
- `TH01_evasion_shadow_copy_delete.jpeg`

**What this showed**
- shadow copy deletion activity, which is a strong sign of defense evasion and anti forensic behavior

## Example KQL approach

A big part of this hunt was not just knowing what to look for, but knowing how to pivot once I found something useful. I used KQL heavily to narrow down attacker behavior by command line, event ID, hostname, and process relationships.

Some of the kinds of searches I used included:
- filtering by `EventCode_s == "1"` for process creation
- filtering by `EventCode_s == "7045"` for suspicious service creation
- searching command lines for terms like `rclone`, `net use`, `copy`, `certutil`, `vssadmin`, and `rundll32`
- pivoting by host name to separate workstation, server, and domain controller activity
- sorting by time to rebuild the attacker timeline

## Biggest takeaways

This hunt was a good example of how an attacker can move through an environment using a mix of:
- built in Windows utilities
- stolen credentials
- common admin style commands
- legitimate looking tooling

The biggest lesson for me was how important it is to slow down and build the timeline correctly. A lot of the answers were not hard because the commands were advanced. They were hard because the attacker activity blended in with a lot of normal looking noise, and the real work was knowing how to filter that noise down into something useful.

## Repository contents

This repo includes:
- screenshots from key parts of the investigation
- a written summary of the attack chain
- examples of the logic I used during the hunt

## Screenshot index

### Overview
- `TH01_overview_attack_scope.jpeg`

### Initial access and execution
- `TH01_initial_certutil_download.jpeg`
- `TH01_initial_payload_execution.jpeg`
- `TH01_exec_rundll32_chain.jpeg`
- `TH01_exec_suspicious_process_tree.jpeg`

### Credential access
- `TH01_cred_lsass_dump.jpeg`
- `TH01_cred_sensitive_process_access.jpeg`

### Discovery and persistence
- `TH01_discovery_user_enum.jpeg`
- `TH01_persist_account_creation.jpeg`
- `TH01_persist_remote_access_tool.jpeg`

### Lateral movement
- `TH01_lat_net_use_auth.jpeg`
- `TH01_lat_share_access.jpeg`
- `TH01_lat_remote_copy.jpeg`

### Staging and exfiltration
- `TH01_stage_archive_creation.jpeg`
- `TH01_stage_data_preparation.jpeg`
- `TH01_exfil_rclone_execution.jpeg`
- `TH01_exfil_mega_connection.jpeg`

### Defense evasion
- `TH01_evasion_shadow_copy_delete.jpeg`

## Final note

This was one of the better hands on investigations I’ve worked through because it forced me to do more than just identify single events. I had to follow the attack from host to host, understand what mattered, and connect everything back into one timeline.

That was the real value of it.
