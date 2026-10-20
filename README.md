# Project 1: Security Onion Network Reconnaissance Detection Lab

## Overview

This project documents a hands-on network security monitoring lab investigating network reconnaissance generated with Nmap and monitored with Security Onion. The lab was built in an isolated VMware environment to practice examining network activity from a defensive perspective.

The project focuses on the investigation workflow, observed telemetry, and packet-level evidence. Findings below are limited to activity and outputs that were recorded during the lab.

## Objectives

- Generate controlled network reconnaissance traffic using Nmap in an isolated lab.
- Observe network activity through Security Onion and its detection telemetry.
- Review packet-level evidence (PCAP) to support investigation findings.
- Correlate scan context, source and destination details, and available alerts.
- Document the lab architecture, results, and limitations clearly.

## Lab Environment

The lab used VMware-based virtual machines and isolated virtual networking.

| Component | Role |
|---|---|
| Kali Linux | Source host used to generate controlled Nmap reconnaissance |
| Security Onion | Network security monitoring and investigation platform |
| Windows target | Destination system used during network monitoring exercises |
| VMware virtual networking | Isolated environment for lab traffic |

### Recorded network details

- Kali source address recorded for the exercise: `192.168.200.10`
- Windows target address recorded for the exercise: `192.168.200.103`
- One recorded alert involved traffic toward TCP destination port `1433`

These values describe the lab as recorded during the exercise. They are private lab addresses, not public service endpoints.

## Investigation Workflow

1. **Prepare the isolated lab.** Confirm the virtual machines and network path used for the exercise.
2. **Generate reconnaissance traffic.** Use Nmap from the Kali VM against the authorized lab target.
3. **Observe the monitoring platform.** Review available Security Onion/Suricata detection telemetry for the test activity.
4. **Inspect supporting evidence.** Use available packet-capture and event details to examine the activity in context.
5. **Record findings and limitations.** Distinguish observed alerts and packet evidence from assumptions about what the monitoring platform detected.

Only systems within the user's own isolated lab were in scope.

## Recorded Evidence and Findings

During a recorded lab exercise, Suricata produced an alert associated with traffic from the Kali host (`192.168.200.10`) toward the Windows target (`192.168.200.103`) on TCP destination port `1433`.

The project uses that recorded alert as an example of network monitoring evidence. The available notes establish that the alert appeared in Suricata telemetry; they do not, by themselves, establish comprehensive detection coverage for every Nmap scan type or every reconnaissance technique.

### Evidence to include

Sanitized screenshots and/or redacted event excerpts should be added to the `evidence/` directory after review. Recommended evidence:

- A view of the lab topology with sensitive or unnecessary details removed.
- A relevant Suricata alert or Security Onion investigation screen.
- A short, sanitized PCAP or packet-analysis screenshot that supports the finding, if available.
- A concise note explaining what each artifact demonstrates.

Do not upload credentials, tokens, externally routable sensitive details, or unreviewed packet captures that may contain unrelated data.

## Security and Scope Notes

- This was a controlled educational lab, not a production security assessment.
- Findings are limited to the recorded test and evidence available.
- An alert is evidence that a detection event was generated; it is not proof that every related packet or scan was detected.
- PCAP evidence and alert telemetry are different sources and should be described separately where applicable.

## Skills Practiced

- Network reconnaissance fundamentals
- Network security monitoring with Security Onion and Suricata
- Alert review and investigation
- Packet-capture analysis workflow
- Evidence-based documentation and reporting
- Safe testing in an isolated virtual lab

## Repository Structure

- `README.md` — project overview, lab context, investigation workflow, and findings.
- `evidence/` — sanitized screenshots and supporting evidence to be added after review.

## Next Steps

- Add sanitized screenshots that directly support the recorded finding.
- Include a concise investigation timeline if timestamps and event details are available.
- Document any additional scan types only after repeating the test and retaining supporting evidence.

## Author

**Taiwo Ibrahim**  
Cybersecurity | Security Operations | Network Security
