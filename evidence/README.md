# Project 1 — Evidence and Investigation Artifacts

This folder contains the network topology, Nmap scan results, Security Onion detection findings, packet-level observations, and investigation summary for the Security Onion Network Reconnaissance Detection Lab.

## Evidence Files

- **01-lab-topology.png** — Network diagram showing the VMware lab configuration, isolated VMnet2 network, VMnet8 management network, and the Kali Linux, Security Onion, and Windows 10 systems.
- **02-investigation-summary-graphic.png** — Summary of the investigation, including the source and target systems, findings, analyst assessment, and conclusion.
- **03-alert-correlation-summary-graphic.png** — Overview of the reconnaissance alerts observed during the exercise and their correlation by source, destination, and time.
- **04-suricata-alert-summary-graphic.png** — Summary of the Suricata detection associated with reconnaissance traffic to common service ports.
- **05-nmap-scan-summary-graphic.png** — Summary of the Nmap TCP SYN scan against the Windows 10 target, including the open and filtered port results.
- **06-security-onion-pcap-job-1002-packet-view.png** — Security Onion packet-table view from PCAP Job 1002, showing TCP traffic from Kali Linux to the Windows target.

## Investigation Context

The exercise was performed in an isolated VMware lab on 24 August 2026. Nmap reconnaissance traffic originated from Kali Linux (`192.168.200.10`) and targeted Windows 10 (`192.168.200.103`). Security Onion and Suricata were used to review network activity, while packet-level observations were correlated with the scan results.

The collected evidence was consistent with TCP SYN reconnaissance. No successful compromise was demonstrated during this exercise.

For the investigation methodology, detailed findings, and analyst assessment, see the [Project 1 case study](../case-study.md) and the [main README](../README.md).

## Scope

This documentation describes the observed activity and findings from the lab exercise. Detection alerts, packet observations, and Nmap results are treated as distinct sources of evidence and interpreted within the limits of the available records.
