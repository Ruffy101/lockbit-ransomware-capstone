# lockbit-ransomware-capstone
A cybersecurity capstone exploring LockBit ransomware, attack lifecycle, security monitoring, and defensive detection in a controlled virtual lab.
# LockBit Ransomware Analysis and Detection Lab

**A Cybersecurity Capstone Project**

## Project Overview

Ransomware is no longer just a program that locks files and demands payment. Many modern campaigns combine unauthorised access, credential theft, movement between systems, data theft, and attempts to interfere with recovery. These activities can disrupt an organisation even before files are encrypted.

LockBit is a useful case study because it operated through a ransomware-as-a-service model. Instead of one team carrying out every stage of every attack, the core operation maintained the ransomware platform and related services while affiliates conducted intrusions. This arrangement helped the operation scale, but it also meant that individual incidents could differ in their tools and techniques (CISA et al., 2023).

The aim of this capstone is to examine LockBit from a defender’s point of view. It reviews the operation’s development, describes a representative attack lifecycle, maps documented behaviours to MITRE ATT&CK, and identifies controls that can help an organisation detect or limit ransomware activity. A small Wazuh exercise is included to show how endpoint telemetry can support an investigation and where basic monitoring has limits.
The research relies primarily on public material from CISA and partner agencies, MITRE ATT&CK, Europol, and the U.S. Department of Justice. These sources provide technical and operational context, while the lab section reports only the evidence collected in the student’s environment.


## Project Objectives

- Aim: To assess LockBit as a ransomware threat and use the findings to develop practical defensive recommendations.

Objectives:

- Describe LockBit’s development and its ransomware-as-a-service business model.
- Explain the main stages commonly associated with a LockBit intrusion.
- Map representative behaviours to MITRE ATT&CK techniques.
- Document a controlled Wazuh simulation and interpret its results accurately.
- Recommend security controls that reduce the likelihood and impact of ransomware incidents.

## Lab Architecture

The lab is designed around a host computer and virtual machines.

| Component | Role |
|---|---|
| macOS MacBook | Host machine running the virtual lab |
| Kali Linux VM | Security testing and assessment machine |
| Ubuntu VM | Target or victim machine for authorized lab exercises |
| Wazuh | Security monitoring and alert analysis, where configured |

## Tools and Technologies

- **Kali Linux:** Security testing in an authorized environment.
- **Ubuntu Linux:** Target system for controlled lab exercises.
- **Wazuh:** Security monitoring, log collection, and alert analysis, if deployed.
- **Virtualization software:** Used to host and isolate virtual machines.
- **MITRE ATT&CK:** Framework for classifying relevant adversary techniques.
- **Markdown and GitHub:** Project documentation and version control.

## Methodology

This study uses qualitative threat-intelligence analysis. It prioritizes primary sources such as government cybersecurity advisories, law-enforcement announcements, and MITRE ATT&CK entries. Secondary reporting is used for context where appropriate.

The work was completed in three stages. First, LockBit was described in terms of its operating model, versions, and history. Second, publicly documented behaviours were organised into a general attack lifecycle. Third, selected behaviours were associated with ATT&CK techniques and considered alongside defensive controls. A separate, non-destructive lab exercise used Wazuh to observe command and file-change activity. The exercise did not use LockBit malware.

## Key Areas of Investigation

- LockBit ransomware and the RaaS model
- Ransomware attack lifecycle
- MITRE ATT&CK technique mapping
- Host-based security monitoring
- Log analysis and alert investigation
- Incident response and recovery
- Security hardening and risk reduction

## Findings and Results

Observed evidence

On 14 October 2026 at 09:24:29 (UTC+1), Wazuh agent 001 on Ubuntu (192.168.64.4) generated rule 5402, titled “Successful sudo to ROOT executed.” The alert was level 3 and belonged to the syslog and sudo groups. 
The event was collected through journald and processed by the Wazuh manager running on Kali.
The user `ruffy`, working from `/dev/tty1` in `/home/ruffy`, ran the following command with root privileges:
/usr/bin/cp /tmp/lockbit-lab/testfile /tmp/lockbit-lab/testfile.lockbit-simulated

The decoder described this as the user’s first recorded sudo use. The event details should be retained as lab evidence, and the timestamp and IP address should be checked against the screenshots or exported Wazuh event before final submission.


### Summary of Results

Interpretation
The command copied a test file and gave the copy a filename containing `.lockbit-simulated`. This imitated a visible file-marking indicator without encrypting the file. No real data was affected. Wazuh detected privileged command execution; the available evidence does not show that Wazuh identified the file operation as ransomware activity.

This distinction limits the conclusion that can be drawn. The event is useful for demonstrating that command telemetry was collected, but it is not proof of a LockBit-specific alert or of successful ransomware detection. The interpretation depends on the command and its test filename, rather than on a dedicated Wazuh classification.


## Project Limitations

- No real LockBit execution: 
The exercise used harmless file creation, copying, and modification. It did not execute LockBit or reproduce the full behaviour of the malware. The results demonstrate basic monitoring, not detection of the actual malware family.

- Limited behaviour coverage: 
The demonstration focused on command telemetry and file changes. It did not test encryption, persistence, credential theft, lateral movement, exfiltration, or backup deletion.

- File-integrity monitoring does not establish intent: 
A file change can be legitimate or malicious. Analysts must correlate it with process information, user context, timing, and other telemetry before drawing conclusions.

- Detection rules were not fully validated: 
The lab showed that relevant events could be observed, but it did not establish that a high-confidence LockBit-specific rule had fired.

- Small, isolated environment: 
Testing involved a small number of virtual systems and a limited set of files. Results may differ in a larger network or under different operating systems, policies, workloads, and agent versions.

- No production incident-response exercise: 
The project did not test enterprise-wide isolation, forensic imaging, scoping, or recovery from backups. Those response actions were discussed rather than tested against a live incident.

## Recommendations

- Build a ransomware detection baseline aligned with MITRE ATT&CK and validate it with safe simulations.

- Require phishing-resistant multifactor authentication for privileged and externally accessible accounts wherever feasible.

- Apply least privilege and separate standard user accounts from administrative accounts.

- Patch internet-facing systems according to a documented, risk-based service-level agreement.

- Segment critical servers, administrative systems, backups, and user networks.

- Centralise endpoint, authentication, DNS, proxy, firewall, and cloud logs so analysts can correlate activity.

- Protect endpoint detection tools and backup infrastructure against tampering.

-Develop detections for credential access, unusual remote services, defence evasion, suspicious data transfers, and rapid file modification.


## Conclusion

LockBit demonstrates how ransomware can operate as a wider criminal service rather than a single piece of malware. Its affiliate model, changing technical capabilities, data-extortion practices, and documented law-enforcement disruption make it a useful subject for a cybersecurity capstone.

The main defensive lesson is to investigate behaviour in context. A suspicious command, an unusual login, or a file change may be important, but none should automatically be treated as proof of ransomware. Analysts need to connect identity, endpoint, network, and file activity, then compare the combined evidence with known attack patterns.

The Wazuh exercise adds practical value because it shows both what the lab could observe and what it could not establish. The system recorded privileged command execution, while the simulated file operation did not demonstrate encryption or a dedicated LockBit detection. Stating these limits clearly makes the findings more credible. Further work should validate file-integrity coverage, add a reproducible event timeline, and test detections using harmless simulations.

## References

CISA, FBI, MS-ISAC, ACSC, CCCS, NCSC-UK, ANSSI, BSI, CERT NZ, & NCSC NZ. (2023, June 14). Understanding ransomware threat actors: LockBit (AA23-165A). Cybersecurity and Infrastructure Security Agency.
Europol. (2024, February 20). Law enforcement disrupts world’s biggest ransomware operation — LockBit was the most deployed ransomware variant across the world. European Union Agency for Law Enforcement Cooperation.
Federal Bureau of Investigation. (2024, February 20). FBI Cyber Deputy Assistant Director Brett Leatherman’s remarks at press conference announcing the disruption of the LockBit ransomware group. U.S. Department of Justice.
MITRE ATT&CK. (2025a). LockBit 2.0 (S1199). MITRE.
MITRE ATT&CK. (2025b). LockBit 3.0 (S1202). MITRE.
MITRE ATT&CK. (2025c). StealBit (S1200). MITRE.
Sophos. (2024). GOLD MYSTIC: Threat profile detail. SophosLabs.
U.S. Department of Justice. (2024a, February 20). U.S. and U.K. disrupt LockBit ransomware variant. U.S. Department of Justice.
U.S. Department of Justice. (2024b, May 7). U.S. charges Russian national with developing and operating LockBit ransomware. U.S. Department of Justice.

## Author

**Ajayi Rafiu Olakunle**

Cybersecurity Student | Aspiring Cybersecurity Analyst

- GitHub: [GitHub Profile](https://github.com/Ruffy101))
- LinkedIn: [LinkedIn Profile](https://www.linkedin.com/)

## Disclaimer

This project is intended for educational, research, and defensive cybersecurity purposes only. All testing must be conducted on authorized systems in a controlled environment. The author does not endorse unauthorized access, malware deployment, or other unlawful activity.
