# Project 1 — Security Onion Network Reconnaissance Detection Lab

This project documents an authorized TCP SYN reconnaissance exercise in an isolated VMware home lab. Kali Linux generated an Nmap scan against a Windows 10 virtual machine; Security Onion/Suricata alerts and packet-level evidence were reviewed and correlated with the Nmap results.

> **Scope:** Lab-only activity against a Windows virtual machine controlled by the author. The evidence in this exercise did not demonstrate successful compromise.

## Investigation at a glance

| Item | Recorded value |
|---|---|
| Date | 24 August 2026 |
| Scan time | Approximately 16:37 WAT |
| Source | Kali Linux — `192.168.200.10` |
| Target | Windows 10 — `192.168.200.103` |
| Monitoring | Security Onion with Suricata |
| Scan command | `sudo nmap -sS -Pn 192.168.200.103` |
| Nmap result | Target up; `5357/tcp` open (`wsdapi`); 999 TCP ports filtered/no response |
| PCAP evidence | Security Onion PCAP Job `1002` |
| ATT&CK mapping | T1046 — Network Service Scanning |

## Lab topology and network configuration

The lab ran in VMware Workstation with two virtual networks:

- **VMnet8 (NAT)** — management network, subnet `192.168.117.0/24`. Security Onion management address: `192.168.117.150`.
- **VMnet2 (Host-only)** — isolated lab network, subnet `192.168.200.0/24`. Used for traffic between Kali, the target, and the Security Onion monitoring interface.
- **Kali Linux:** `192.168.200.10`, used to initiate the scan.
- **Windows 10 target:** `192.168.200.103`.
- **Security Onion monitoring interface:** connected to VMnet2 without an IP address assigned to that sniffing interface; management access was through VMnet8 at `192.168.117.150`.

The corrected topology diagram is the source of truth for the VM roles and addresses. The lab VMs did not require direct Internet access for this exercise.

## Method

1. Prepared the isolated lab topology and confirmed the source and target addresses.
2. Ran a TCP SYN scan from Kali:
   ```bash
   sudo nmap -sS -Pn 192.168.200.103
   ```
3. Reviewed Security Onion/Suricata detection telemetry for traffic between the source and target.
4. Inspected packet-level evidence from PCAP Job 1002.
5. Correlated timestamps, source and destination addresses, TCP SYN flags, destination ports, alerts, and the Nmap output.
6. Recorded the analyst assessment and considered whether the evidence showed compromise.

## Nmap results

The recorded Nmap output showed:

- Target host was up.
- `5357/tcp` was open; service label `wsdapi`.
- 999 TCP ports were filtered/no response.
- Scan duration was approximately 18.32 seconds.

These results describe the observed output for this particular scan; they do not establish the status of every service or the result of other scan types.

## Security Onion / Suricata detections

The investigation notes record multiple ET SCAN alerts, including:

- ET SCAN Suspicious inbound to MSSQL port `1433`
- ET SCAN Suspicious inbound to Oracle SQL port `1521`
- ET SCAN Suspicious inbound to PostgreSQL port `5432`
- ET SCAN Suspicious inbound to mySQL port `3306`
- ET SCAN Potential VNC Scan `5800–5820`

One detailed Suricata event was recorded for traffic from `192.168.200.10` to `192.168.200.103:1433` over TCP. The event was categorized as “Potentially Bad Traffic,” with severity `2` and action `allowed`. This indicates traffic matched a reconnaissance signature; it does **not** prove that MSSQL was available or compromised.

## PCAP review and correlation

The notes record that PCAP Job `1002` completed successfully. The packet view showed repeated TCP SYN probes from `192.168.200.10` to `192.168.200.103`; destination ports visible in the supplied notes included `22`, `111`, `143`, `445`, `8080`, `3389`, and `8888`, with further probes present in the capture.

| Evidence source | What it supported |
|---|---|
| Nmap terminal output | The scan command and observed open/filtered port results |
| Security Onion / Suricata alerts | Reconnaissance signatures matched activity involving the source and target |
| PCAP view | TCP SYN probes between the same lab hosts |
| Correlation of time and addresses | The evidence was consistent with the authorized reconnaissance exercise |

The different evidence sources are complementary. Alert telemetry is not the same as packet-level evidence, and neither should be overstated.

## Analyst assessment

- **Classification:** Network reconnaissance / TCP SYN scanning.
- **Confidence:** High that the observed traffic corresponds to the authorized Nmap scan, based on the recorded command, time window, addresses, alerts, and PCAP observations.
- **Compromise assessment:** No evidence collected in this exercise demonstrated successful compromise, payload delivery, or lateral movement.
- **MITRE ATT&CK:** T1046 — Network Service Scanning.

## Recommended response if observed in an enterprise

1. Confirm whether the scanning source is an authorized scanner.
2. Identify the target asset and its owner.
3. Determine whether the scan is expected and in scope.
4. Review network and endpoint telemetry for follow-on activity.
5. If unauthorized, investigate and contain according to the organization’s incident-response procedures.
6. Document scope, evidence, findings, actions, and remaining uncertainties.

## Skills demonstrated

Network reconnaissance fundamentals · Suricata alert triage · Security Onion investigation · PCAP review · Nmap validation · Evidence correlation · MITRE ATT&CK mapping · SOC reporting

## Repository contents

- `README.md` — project summary, topology, method, findings, and assessment.
- `case-study.md` — detailed case study.
- `evidence/README.md` — evidence index and evidence-handling guidance.
- `evidence/` — sanitized project artifacts as they are reviewed and added.

## Limitations and evidence handling

This is an educational home-lab exercise, not a production security assessment. The results are limited to the recorded scan and evidence. A Suricata signature match does not independently prove a service was running, a host was compromised, or all scan traffic was detected. No raw PCAP is published here. Before publishing screenshots, inspect them for credentials, tokens, unrelated network traffic, unnecessary host details, and any information not intended for public release.

## Author

**Taiwo Ibrahim**  
Cybersecurity | Security Operations | Network Security
