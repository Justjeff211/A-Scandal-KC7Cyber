
# A-Scandal-KC7Cyber

This project documents my investigation of a phishing-driven compromise in the *“A Scandal”* scenario from KC7Cyber.

It reflects my growing ability to move beyond isolated queries and think in terms of attacker behaviour, event correlation, and end-to-end incident analysis.

---

## Project Overview

The investigation follows a real-world attack simulation where an employee interacts with a phishing email, leading to a full system compromise.

Using KQL, I reconstructed the attack timeline by correlating:

- Network activity (initial access)
- File creation events (payload delivery)
- Process execution logs (attacker behaviour)

---

## Key Highlights

- Identified the exact moment the phishing link was clicked  
- Tracked the download of a malicious document  
- Detected PowerShell-based payload execution  
- Uncovered remote access activity using plink  
- Observed post-exploitation discovery commands  
- Correlated multiple log sources to build a full attack chain  
