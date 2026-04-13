
# A Scandal | KC7Cyber

## Overview

This investigation simulates a phishing-driven compromise involving an employee, Ronnie McLovin. The objective was to reconstruct the attack timeline using KQL across multiple data sources.

As someone preparing for SC-200, this exercise helped me move beyond running isolated queries and start thinking in terms of attack flow and correlation.

----------

## Scenario Summary

A phishing email containing a malicious link was sent to an employee. After interaction, the system was compromised, leading to payload execution, remote access and post-exploitation activity.

----------

## Investigation Process

### 1. Initial Access - Phishing Link Click

```kql
OutboundNetworkEvents
| where src_ip == "10.10.0.19"
| where url contains "promotionrecruit"
| project timestamp, src_ip, url

```

This query identified the exact moment the user interacted with the phishing link, establishing the starting point of the attack timeline.

----------

### 2. Host Identification

```kql
Employees
| where name contains "Ronnie"

```

Mapped the user to their endpoint:

-   **Hostname:** A37A-DESKTOP
    

----------

### 3. Payload Delivery - Malicious Document

```kql
FileCreationEvents
| where hostname contains "A37A-DESKTOP"
| where filename contains "Editorial"

```

This revealed when the malicious document was downloaded, marking the transition from initial access to execution.

----------

### 4. Script Execution - PowerShell Payload

```kql
FileCreationEvents
| where hostname contains "A37A-DESKTOP"
| where filename contains ".ps1"

```

Confirmed the presence of a PowerShell script, indicating payload staging and execution.

----------

### 5. Remote Access - plink Usage

```kql
ProcessEvents
| where hostname contains "A37A-DESKTOP"
| where process_commandline contains "plink"

```

Findings:

-   Remote IP: **168.57.191.100**
    
-   Credentials exposed in command line
    

This indicates the establishment of a remote access tunnel.

----------

### 6. Post-Exploitation - Discovery Activity

Multiple discovery commands (5 total) were executed, suggesting enumeration of the compromised environment.

----------

### 7. Additional Malicious Activity

```kql
OutboundNetworkEvents
| where src_ip == "10.10.0.19"
| where url contains "fakestory.docx"

```

Tracked further suspicious outbound activity, reinforcing the attack chain.

----------

## Key Takeaways

-   Correlation across data sources is critical:
    
    -   Network logs > Initial access
        
    -   File events > Payload staging
        
    -   Process logs > Attacker behaviour
        
-   Building a timeline is more valuable than isolated findings.
    
-   Investigation requires persistence. Some queries returned no results initially, reinforcing the need to validate assumptions and remain methodical.
    

----------


## Snapshots of a few KQL queries ran
https://github.com/user-attachments/assets/275b3b7e-1d91-4717-9897-f028a66c2278
https://github.com/user-attachments/assets/58887d44-7212-45a6-9a65-4bd59df45506
https://github.com/user-attachments/assets/d39a9744-476e-4e7e-9300-6c1ffdc5e1fc
https://github.com/user-attachments/assets/3a2bd1fa-0fa9-42cc-b067-025f75744ff6
https://github.com/user-attachments/assets/fce9f2e1-180c-4bbf-97c3-40fedcbd9926
https://github.com/user-attachments/assets/0d4626fe-b0d3-44e7-9bd1-9fcfc763de53
https://github.com/user-attachments/assets/655c4155-1a5f-4779-9115-19d257a1d410
https://github.com/user-attachments/assets/8a259d98-6aae-4234-b074-e8c6aac36d2b

----------

## Reflection

This exercise highlighted the importance of patience and analytical thinking. As someone early in the journey toward SC-200, it reinforced that developing an investigative mindset is just as important as learning the tools.

----------
