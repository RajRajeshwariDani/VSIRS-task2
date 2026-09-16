# Week 2 — Virtual Security Incident Response Simulation

## 📌 Project Overview

This project was completed as part of my cybersecurity internship as a hands-on **Virtual Security Incident Response Simulation**.

The purpose of this exercise was to simulate a security incident in a controlled Windows virtual machine and practice the complete incident-response lifecycle used by SOC and cybersecurity teams.

The simulation covered:

- Incident preparation
- Security incident detection
- Incident analysis
- Indicator of Compromise (IoC) identification
- Evidence collection and preservation
- Containment
- Eradication
- Recovery
- Final validation
- Post-incident analysis
- Security improvement recommendations

> **Important:** This was a controlled training simulation. No real malware, production systems, unauthorized targets, or real-world attacks were involved.

---

# 🎯 Objectives

The main objectives of this task were:

1. Simulate a security incident in an isolated virtual environment.
2. Identify suspicious artifacts and persistence mechanisms.
3. Investigate processes, network connections, scheduled tasks and Windows events.
4. Collect and preserve evidence.
5. Develop and execute a containment procedure.
6. Remove the simulated persistence and artifact.
7. Perform recovery and final validation.
8. Document the incident using a recognized incident-response framework.
9. Identify security gaps and recommend preventive controls.
10. Prepare a professional post-incident report.

---

# 🖥️ Lab Environment

## Operating Environment

- Windows Virtual Machine
- Administrator PowerShell
- Isolated/controlled virtual environment
- Local evidence directory: `C:\IR-Lab`

## Tools Used

- Windows PowerShell
- Windows Event Viewer / Windows Event Logs
- Windows Task Scheduler
- `Get-Process`
- `netstat`
- `Get-FileHash`
- `Get-ScheduledTask`
- `Get-WinEvent`
- Windows file-system commands

---

# 🔐 Simulated Incident Scenario

A phishing-style endpoint compromise was simulated without using real malware.

A harmless text file named:

`dropper_sim.txt`

was created to represent a suspicious payload artifact.

A scheduled task named:

`UpdateCheck-Simulation`

was created to represent a persistence mechanism.

The simulated incident flow was:

```text
Simulated Phishing Event
        ↓
User Execution
        ↓
Simulated Artifact
        ↓
Simulated Persistence
        ↓
Detection
        ↓
Investigation
        ↓
Evidence Preservation
        ↓
Containment
        ↓
Eradication
        ↓
Recovery
        ↓
Final Validation
        ↓
Post-Incident Analysis
🧪 Hands-On Practical Work
1. Created the Evidence Directory
The investigation started by creating a dedicated evidence directory.
New-Item -ItemType Directory -Force C:\IR-Evidence
New-Item -ItemType Directory -Force C:\IR-Evidence\Screenshots
New-Item -ItemType Directory -Force C:\IR-Evidence\Logs
New-Item -ItemType Directory -Force C:\IR-Evidence\Artifacts
This provides a structured location for storing investigation evidence.
2. Identified the Windows Host
The hostname was collected using:
hostname
System information was collected using:
systeminfo | Select-String "OS Name","OS Version","System Type"
Network configuration was checked using:
ipconfig
Active network connections were reviewed using:
netstat -ano
These commands establish the basic identity and network context of the investigated endpoint.
3. Created the Simulated Incident Artifact
A harmless simulated artifact was created:
New-Item -ItemType Directory -Force C:\IR-Lab | Out-Null
Set-Content C:\IR-Lab\dropper_sim.txt "SIMULATED INCIDENT ARTIFACT - NO MALWARE"
The artifact was then inspected:
Get-Item C:\IR-Lab\dropper_sim.txt
Security Purpose
This represented a suspicious file discovered during an endpoint investigation.
The file contained no malware and was used only for training.
4. Created Simulated Persistence
A harmless scheduled task was created to represent persistence:
$action = New-ScheduledTaskAction -Execute "cmd.exe" -Argument "/c echo SIMULATED-PERSISTENCE > C:\IR-Lab\persistence_check.txt"

$trigger = New-ScheduledTaskTrigger -Once -At (Get-Date).AddMinutes(2)

Register-ScheduledTask -TaskName "UpdateCheck-Simulation" -Action $action -Trigger $trigger -Force
The scheduled task was verified:
Get-ScheduledTask -TaskName "UpdateCheck-Simulation"
The task appeared in the Ready state.
Security Purpose
Scheduled tasks can be investigated during real endpoint investigations because attackers may abuse legitimate operating-system mechanisms for persistence.
In this exercise, the task was completely harmless.
5. Detection
The simulated persistence mechanism was searched for:
Get-ScheduledTask |
Where-Object {$_.TaskName -like "*Simulation*"} |
Select-Object TaskName,State
The simulated task:
UpdateCheck-Simulation
was identified.
This demonstrated the detection phase of the incident-response process.
6. Artifact Investigation
The simulated artifact was inspected:
Get-ChildItem C:\IR-Lab
A SHA-256 hash was calculated:
Get-FileHash C:\IR-Lab\dropper_sim.txt -Algorithm SHA256
The hash was saved as evidence:
Get-FileHash C:\IR-Lab\dropper_sim.txt -Algorithm SHA256 |
Out-File C:\IR-Lab\artifact_hash.txt
Security Purpose
Hashing helps identify an artifact and provides an integrity reference for evidence.
7. Process Investigation
Running processes were collected:
Get-Process |
Sort-Object CPU -Descending |
Select-Object -First 10 Name,Id,CPU
PowerShell processes were specifically investigated:
Get-Process powershell,pwsh -ErrorAction SilentlyContinue |
Select-Object Name,Id,Path
The results were preserved:
Get-Process |
Select-Object Name,Id,Path |
Out-File C:\IR-Lab\processes.txt
Security Purpose
Process investigation helps an analyst identify unusual processes and understand what is running on the affected endpoint.
8. Network Investigation
Active network connections were collected:
netstat -ano |
Out-File C:\IR-Lab\network_connections.txt
This information can be correlated with processes and timestamps during an incident investigation.
9. Scheduled Task Evidence
The simulated scheduled task information was preserved:
Get-ScheduledTask -TaskName "UpdateCheck-Simulation" |
Format-List * |
Out-File C:\IR-Lab\scheduled_task.txt
This created a record of the simulated persistence mechanism before eradication.
10. Windows Event Evidence
Recent Windows Security events were exported:
Get-WinEvent -LogName Security -MaxEvents 30 |
Select-Object TimeCreated,Id,ProviderName,Message |
Out-File C:\IR-Lab\security_events.txt
Recent System events were exported:
Get-WinEvent -LogName System -MaxEvents 30 |
Select-Object TimeCreated,Id,ProviderName,Message |
Out-File C:\IR-Lab\system_events.txt
Security Purpose
Windows event logs provide useful information for investigating authentication, system and security activity.
11. Evidence Collection
The following evidence was collected:
processes.txt
network_connections.txt
scheduled_task.txt
artifact_hash.txt
security_events.txt
system_events.txt
incident_timeline.txt
CONTAINMENT_NOTE.txt
RECOVERY_COMPLETED.txt
EVIDENCE_MANIFEST.txt
The evidence directory was checked using:
Get-ChildItem C:\IR-Lab
12. Simulated Containment
Containment was documented in the controlled virtual environment.
New-Item -ItemType File -Path C:\IR-Lab\CONTAINMENT_COMPLETED.txt -Force
A containment note was created:
Set-Content C:\IR-Lab\CONTAINMENT_NOTE.txt "Simulated containment completed. Affected virtual workstation isolated from the lab network."
Security Purpose
The purpose of containment is to prevent further impact while preserving evidence.
In a real enterprise environment, containment could involve approved EDR isolation, network segmentation, firewall controls or NAC mechanisms.
13. Eradication
The simulated persistence mechanism was removed:
Unregister-ScheduledTask -TaskName "UpdateCheck-Simulation" -Confirm:$false
The harmless simulated artifact was removed:
Remove-Item C:\IR-Lab\dropper_sim.txt -Force
The artifact was verified:
Test-Path C:\IR-Lab\dropper_sim.txt
Expected result:
False
The scheduled task was checked:
Get-ScheduledTask -TaskName "UpdateCheck-Simulation" -ErrorAction SilentlyContinue
No result indicated that the simulated persistence had been removed.
14. Recovery
Recovery was documented using:
Set-Content C:\IR-Lab\RECOVERY_COMPLETED.txt "System restored to a known-good simulated state. Persistence and simulated artifact removed. Validation completed."
The evidence directory was reviewed again:
Get-ChildItem C:\IR-Lab
The recovery phase represents restoring the endpoint to a trusted state after eradication.
15. Final Validation
The final artifact check was performed:
Write-Host "=== ARTIFACT CHECK ==="
Test-Path C:\IR-Lab\dropper_sim.txt

Write-Host "=== PERSISTENCE CHECK ==="
Get-ScheduledTask -TaskName "UpdateCheck-Simulation" -ErrorAction SilentlyContinue

Write-Host "=== EVIDENCE FILES ==="
Get-ChildItem C:\IR-Lab | Select-Object Name,Length
Expected validation
=== ARTIFACT CHECK ===
False

=== PERSISTENCE CHECK ===
No output

=== EVIDENCE FILES ===
Evidence files listed
This confirmed:
Simulated artifact removed
Simulated persistence removed
Evidence preserved
Recovery documentation completed
The PowerShell process visible during final validation represents the analyst's active administrative session and is not treated as malicious persistence.
16. Incident Timeline
The following timeline was documented:
14:00 - Lab preparation completed.
14:05 - Suspicious simulated activity identified.
14:07 - Simulated artifact and persistence mechanism investigated.
14:10 - Affected virtual workstation placed in simulated containment.
14:15 - Process, network, task and artifact evidence preserved.
14:25 - Simulated persistence and artifact removed.
14:35 - Recovery procedure documented.
14:45 - Final validation completed.
15:00 - Post-incident analysis and recommendations prepared.
17. Evidence Manifest
The evidence manifest documented:
WEEK 2 INCIDENT RESPONSE - EVIDENCE MANIFEST

Environment: Controlled Windows Virtual Machine
Incident: Simulated endpoint compromise

Evidence collected:
- Process information
- Network connections
- Scheduled task information
- SHA-256 artifact hash
- Windows Security events
- Windows System events
- Incident timeline
- Containment documentation
- Recovery documentation

Final status:
Simulated artifact removed.
Simulated persistence removed.
Validation completed.
No real malware or production system was involved.
18. Indicators of Compromise
Indicator
Evidence
Interpretation
dropper_sim.txt
C:\IR-Lab\dropper_sim.txt
Harmless simulated payload artifact
UpdateCheck-Simulation
Windows Scheduled Task
Simulated persistence
SHA-256 hash
artifact_hash.txt
Artifact identification/integrity
Process information
processes.txt
Endpoint activity evidence
Network connections
network_connections.txt
Network investigation evidence
Security events
security_events.txt
Event/timeline evidence
System events
system_events.txt
System activity evidence
19. NIST-Aligned Incident Response Lifecycle
Preparation
Prepare the virtual machine.
Establish evidence directories.
Verify logging.
Maintain a known-good recovery point.
Prepare an incident-response checklist.
Detection and Analysis
Identify suspicious activity.
Investigate processes.
Investigate files.
Investigate scheduled tasks.
Review network connections.
Review Windows events.
Calculate artifact hashes.
Determine the affected endpoint and scope.
Containment
Isolate the affected virtual workstation.
Prevent additional activity.
Preserve evidence.
Document containment actions.
Eradication
Remove simulated persistence.
Remove the simulated artifact.
Verify removal.
Address the simulated cause.
Recovery
Restore or rebuild from a known-good state.
Validate system integrity.
Confirm persistence is absent.
Increase monitoring.
Post-Incident Activity
Document findings.
Identify security gaps.
Record lessons learned.
Recommend corrective actions.
Update incident-response procedures.
20. Post-Incident Analysis
Security Gap 1 — Limited Centralized Logging
Problem
Endpoint, authentication and network information may be difficult to correlate when logs are stored separately.
Recommendation
Centralize relevant security telemetry using a SIEM and create appropriate detection rules.
Security Gap 2 — Limited Application Control
Problem
Unapproved scripts or applications could potentially execute on an endpoint.
Recommendation
Use application control, allow-listing and least-privilege principles.
Security Gap 3 — Network Segmentation
Problem
Unnecessary network reachability can increase the potential impact of a compromised endpoint.
Recommendation
Separate user, server and sensitive networks and restrict unnecessary outbound traffic.
Security Gap 4 — Credential Protection
Problem
Compromised credentials can increase the scope of an incident.
Recommendation
Use unique credentials, MFA where applicable, privileged-access controls and credential rotation.
Security Gap 5 — Incident Response Readiness
Problem
Without practiced procedures, analysts may respond inconsistently.
Recommendation
Maintain documented response playbooks and conduct regular tabletop and technical exercises.
21. Lessons Learned
Evidence should be preserved before destructive remediation when investigation requires it.
Process, file, task, network and event information should be correlated into a timeline.
Controlled simulations provide a safe way to practice incident-response procedures.
Known-good virtual-machine snapshots can support recovery.
Security findings should be converted into tracked corrective actions.
Clear documentation is an important part of incident response.
Final validation is necessary before considering recovery complete.
22. Corrective Action Plan
Priority
Corrective Action
High
Centralize endpoint, authentication and network telemetry
High
Deploy appropriate EDR/security monitoring
High
Apply least privilege
Medium
Enable appropriate PowerShell and endpoint logging
Medium
Improve network segmentation
Medium
Use MFA for privileged access where applicable
Medium
Maintain golden VM images/snapshots
Medium
Conduct recurring incident-response exercises
23. Evidence Screenshots
The repository contains screenshots demonstrating the practical work.
Screenshot 1 — Simulated Artifact and Persistence
The screenshot shows creation of the harmless dropper_sim.txt artifact and the UpdateCheck-Simulation scheduled task.
Screenshot 2 — Evidence Collection
The screenshot shows the C:\IR-Lab directory containing investigation outputs such as process, network, scheduled-task and hash evidence.
Screenshot 3 — Final Validation
The screenshot shows the artifact check returning:
False
and the scheduled-task check returning no result.
This demonstrates successful simulated eradication.
Screenshot 4 — Evidence Manifest
The screenshot shows the evidence manifest describing the environment, evidence collected and final status.
Screenshot 5 — Incident Timeline
The screenshot shows the documented incident-response timeline and final validation checks.
24. Repository Structure
Week-2-Virtual-Incident-Response/
│
├── README.md
│
├── Report/
│   ├── Week_2_Incident_Response_Final_Hands_On_Report.pdf
│   └── Week_2_Incident_Response_Final_Hands_On_Report.docx
│
├── Evidence/
│   ├── Screenshots/
│   │   ├── 01_simulated_artifact.jpg
│   │   ├── 02_evidence_collection.jpg
│   │   ├── 03_final_validation.jpg
│   │   ├── 04_evidence_manifest.jpg
│   │   └── 05_incident_timeline.jpg
│   │
│   └── Logs/
│       ├── processes.txt
│       ├── network_connections.txt
│       ├── scheduled_task.txt
│       ├── artifact_hash.txt
│       ├── security_events.txt
│       └── system_events.txt
│
└── Documentation/
    └── incident_timeline.txt
25. Key Commands Used
hostname

systeminfo | Select-String "OS Name","OS Version","System Type"

ipconfig

netstat -ano

New-Item -ItemType Directory -Force C:\IR-Lab

Set-Content C:\IR-Lab\dropper_sim.txt "SIMULATED INCIDENT ARTIFACT - NO MALWARE"

Get-Item C:\IR-Lab\dropper_sim.txt

Get-ScheduledTask -TaskName "UpdateCheck-Simulation"

Get-ChildItem C:\IR-Lab

Get-FileHash C:\IR-Lab\dropper_sim.txt -Algorithm SHA256

Get-Process | Sort-Object CPU -Descending | Select-Object -First 10 Name,Id,CPU

Get-Process powershell,pwsh -ErrorAction SilentlyContinue | Select-Object Name,Id,Path

netstat -ano | Out-File C:\IR-Lab\network_connections.txt

Get-ScheduledTask -TaskName "UpdateCheck-Simulation" |
Format-List * |
Out-File C:\IR-Lab\scheduled_task.txt

Get-WinEvent -LogName Security -MaxEvents 30 |
Select-Object TimeCreated,Id,ProviderName,Message |
Out-File C:\IR-Lab\security_events.txt

Get-WinEvent -LogName System -MaxEvents 30 |
Select-Object TimeCreated,Id,ProviderName,Message |
Out-File C:\IR-Lab\system_events.txt

Unregister-ScheduledTask -TaskName "UpdateCheck-Simulation" -Confirm:$false

Remove-Item C:\IR-Lab\dropper_sim.txt -Force

Test-Path C:\IR-Lab\dropper_sim.txt

Get-ScheduledTask -TaskName "UpdateCheck-Simulation" -ErrorAction SilentlyContinue

Get-ChildItem C:\IR-Lab
26. Final Result
The simulated incident-response exercise was completed successfully.
[✓] Preparation
[✓] Detection
[✓] Investigation
[✓] IoC Identification
[✓] Evidence Preservation
[✓] Containment
[✓] Eradication
[✓] Recovery
[✓] Final Validation
[✓] Post-Incident Analysis
[✓] Lessons Learned
[✓] Corrective Actions
[✓] Documentation
27. Conclusion
This hands-on exercise provided practical experience with the incident-response lifecycle in a controlled virtual environment.
The simulation demonstrated how a cybersecurity analyst can identify suspicious endpoint activity, investigate relevant evidence, document the incident, contain the affected system, remove simulated persistence, recover the system and verify that the simulated threat has been eliminated.
The exercise also highlighted the importance of centralized logging, endpoint monitoring, least privilege, network segmentation, credential protection and regularly tested incident-response procedures.
⚠️ Important Disclaimer
This project is a controlled cybersecurity training simulation.
The file dropper_sim.txt was intentionally harmless.
The scheduled task UpdateCheck-Simulation was intentionally created for demonstrating persistence detection and was subsequently removed.
No real malware was used.
No production system was attacked.
No unauthorized system was accessed.
The project was performed only for educational and internship training purposes.
📚 References
NIST SP 800-61 Rev. 3 — Incident Response Recommendations and Considerations for Cybersecurity Risk Management.
NIST Incident Response Project.
CISA Cybersecurity Incident Response Playbooks.
👩‍💻 Author
Rajeshwari Dani
Cybersecurity | SOC | Incident Response | VAPT
#Cybersecurity #SOC #IncidentResponse #NIST #DigitalForensics #CyberSecurityInternship #WindowsSecurity #PowerShell
