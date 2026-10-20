# SOC Investigation Case Study: TCP SYN Reconnaissance Detection

## Executive summary

This case study records an authorized TCP SYN reconnaissance exercise in an isolated VMware home lab. A scan was generated from Kali Linux against a Windows 10 virtual machine. Security Onion/Suricata alerts were investigated, a PCAP view was reviewed, and the evidence was compared with the Nmap output.

**Workflow:** Generate activity → Detect → Investigate → Correlate → Validate → Document.

**Scope:** The target was a virtual machine controlled by the author. The evidence collected for this exercise did not demonstrate successful compromise.

## Lab environment

| Component | Role | Address / details |
|---|---|---|
| Kali Linux | Reconnaissance source | `192.168.200.10` |
| Windows 10 | Target | `192.168.200.103` |
| Security Onion | Monitoring / SIEM | Management: `192.168.117.150` |
| Suricata | Network IDS | Runs in Security Onion |
| Network | Isolated lab | VMnet2 host-only, `192.168.200.0/24` |
| Management | Security Onion access | VMnet8 NAT, `192.168.117.0/24` |
| PCAP job | Packet evidence | `1002` |

The corrected topology shows Security Onion management on VMnet8 and the monitoring/sniffing interface connected to VMnet2 without an assigned IP address. Lab VMs did not require direct Internet access for this exercise.

## Reconnaissance activity and Nmap results

The supplied notes record a TCP SYN scan from Kali to `192.168.200.103` on 24 August 2026 at approximately 16:37 WAT.

The Nmap output reported:
- Target host was up.
- `5357/tcp` was open, service label `wsdapi`.
- 999 TCP ports were filtered / received no response.
- Duration was approximately 18.32 seconds.

These are results for the recorded scan only, not a complete inventory of the target.

## Security Onion / Suricata findings

The supplied investigation notes list these ET SCAN alerts:
- Suspicious inbound to MSSQL port `1433`
- Suspicious inbound to Oracle SQL port `1521`
- Suspicious inbound to PostgreSQL port `5432`
- Suspicious inbound to MySQL port `3306`
- Potential VNC scan `5800–5820`

A detailed Suricata event was recorded for TCP traffic from `192.168.200.10` to `192.168.200.103:1433`. The supplied notes identify the category as “Potentially Bad Traffic”, severity `2`, action `allowed`, and rule ID `2010935`.

A signature match is evidence of traffic matching a reconnaissance rule. It does not establish that MSSQL was running, exposed, or compromised.

## Packet-level review and correlation

The supplied notes state that PCAP Job `1002` completed and that the packet view showed repeated TCP SYN probes from `192.168.200.10` to `192.168.200.103`. Ports visible in the supplied packet view included `22`, `111`, and `8888`; the notes also record probes involving `143`, `445`, `8080`, and `3389`. The Suricata event excerpt separately showed TCP SYN traffic to port `3306` and a scan signature for `1521`.

| Evidence source | Supported finding |
|---|---|
| Nmap output | The scan activity and observed open/filtered port results |
| Suricata alerts | Reconnaissance signatures matched traffic involving the source and target |
| PCAP view | TCP SYN probes between the same lab hosts |
| Timestamp and address correlation | Artifacts were consistent with the authorized scan |

The timing, source and destination addresses, alerts, packet observations, and Nmap results support high confidence that the activity corresponded to the authorized scan.

## Analyst assessment

- **Classification:** Network reconnaissance / TCP SYN scanning.
- **Confidence:** High that the observed traffic matched the authorized exercise.
- **Impact:** Reconnaissance activity only, within the evidence collected.
- **MITRE ATT&CK:** T1046 — Network Service Scanning.
- **Compromise:** No evidence collected in this exercise demonstrated successful compromise, payload delivery, or lateral movement.

The conclusion is limited to the reviewed evidence. It is not proof that every possible attack path was tested.

## SOC workflow demonstrated

1. Generate a controlled scan within the isolated lab.
2. Review Security Onion / Suricata alerts.
3. Extract source, destination, protocol, timestamp and port details.
4. Correlate alert telemetry with packet-level observations.
5. Compare the findings with Nmap output.
6. Assess whether the available evidence demonstrates compromise.
7. Document findings, limitations and recommended next steps.

## Recommended enterprise response

For similar activity in an enterprise environment: confirm whether the scanner is authorized; identify the target and asset owner; determine whether the scan is expected; review network and endpoint telemetry for follow-on activity; investigate and contain unauthorized activity under established incident-response procedures; and document evidence, decisions and uncertainty.

## Lessons learned

- An alert begins an investigation; it is not the conclusion.
- Cross-source timestamp and address correlation increases confidence.
- Alert telemetry and packet evidence are separate sources.
- Reconnaissance does not automatically mean compromise.
- Conclusions must stay within the limits of the collected evidence.

## Skills demonstrated

Network monitoring, Suricata alert triage, Security Onion investigation, PCAP review, Nmap validation, evidence correlation, MITRE ATT&CK mapping and SOC reporting.

## Scope and limitations

This was an authorized educational home-lab exercise, not a production assessment. The findings apply to the recorded scan and supplied artifacts. A signature match does not prove a service was available or a host was compromised. No raw PCAP is included. Before publishing artifacts, review them for credentials, tokens, unrelated traffic, or unnecessary sensitive details.

## Evidence index

See [evidence/README.md](evidence/README.md). The topology diagram is a corrected diagram, and some supplied summary panels are designed graphics rather than unmodified application screenshots. The packet-table view is a Security Onion interface image. Each artifact should be described according to what it actually demonstrates.

## Author

**Taiwo Ibrahim** — Cybersecurity | Security Operations | Network Security
