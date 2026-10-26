<div align="center">

<h1>Zero Trust UTM Security Lab</h1>
<h3>Enterprise-grade 5-machine cybersecurity infrastructure: network, identity, intelligence, deception and endpoint</h3>

<br>

<img src="https://img.shields.io/badge/MACHINES-5-0d1117?style=for-the-badge&labelColor=0d1117&color=00b4d8" />
<img src="https://img.shields.io/badge/STATUS-4%20OF%205%20COMPLETE-0d1117?style=for-the-badge&labelColor=0d1117&color=00b4d8" />
<img src="https://img.shields.io/badge/ARCHITECTURE-ZERO%20TRUST-0d1117?style=for-the-badge&labelColor=0d1117&color=00b4d8" />
<img src="https://img.shields.io/badge/CLUSTER%20RAM-104%20GB-0d1117?style=for-the-badge&labelColor=0d1117&color=00b4d8" />
<img src="https://img.shields.io/badge/LICENSE-CC%20BY--NC--ND-0d1117?style=for-the-badge&labelColor=0d1117&color=00b4d8" />

<br><br>

<a href="#overview"><b>Overview</b></a> &nbsp;·&nbsp;
<a href="#machines"><b>Machines</b></a> &nbsp;·&nbsp;
<a href="#how-the-machines-work-together"><b>Integration</b></a> &nbsp;·&nbsp;
<a href="#key-results"><b>Results</b></a> &nbsp;·&nbsp;
<a href="#technology-stack"><b>Stack</b></a> &nbsp;·&nbsp;
<a href="#documentation"><b>Documentation</b></a> &nbsp;·&nbsp;
<a href="#repository-structure"><b>Structure</b></a> &nbsp;·&nbsp;
<a href="#project-team"><b>Team</b></a>

</div>

<br>

---

## Overview

Zero Trust UTM is a multi-machine cybersecurity project built on one principle: **never trust, always verify, always monitor, always respond.** It runs across a cluster of 5 physical laptops joined by a Tailscale VPN mesh, with Machine 1 acting as the hub. Each machine owns one plane of the architecture, and together they form a segmented network with threat detection, identity verification, deception technology and endpoint hardening.

> The lab is a private, isolated environment built for study. All IP addresses and credentials shown are lab-only values.

<table>
<tr><th>Item</th><th>Detail</th></tr>
<tr><td><b>Project</b></td><td>Zero Trust UTM, a 5-machine cybersecurity lab</td></tr>
<tr><td><b>Principle</b></td><td>Never trust, always verify, always monitor, always respond</td></tr>
<tr><td><b>Cluster</b></td><td>5 physical laptops, 104 GB combined RAM</td></tr>
<tr><td><b>Interconnect</b></td><td>Tailscale VPN mesh with M1 as the hub</td></tr>
<tr><td><b>Design</b></td><td>Defense in depth, zone-based segmentation, full visibility, automated response</td></tr>
<tr><td><b>Documentation</b></td><td>Project report (PDF), presentation (PPT) and demonstration videos</td></tr>
</table>

<br>

## Machines

Each machine has its own folder with a README, screenshots and the evidence for that plane.

<table>
<tr><th>Machine</th><th>Plane</th><th>RAM</th><th>Key Services</th><th>Status</th></tr>
<tr><td><b><a href="./M1-Core-Network-and-Security/README.md">M1</a></b></td><td>Core Network and Security</td><td>24 GB</td><td>VyOS, FortiGate, Suricata, Pi-hole, Zeek + RITA, Arkime, Traefik + Squid, NGINX, MailCow</td><td>Completed</td></tr>
<tr><td><b><a href="./M2-Identity-and-PKI/README.md">M2</a></b></td><td>Identity and PKI</td><td>16 GB</td><td>Samba AD, Keycloak, OpenLDAP, FreeRADIUS, Vault, Step-CA, EJBCA, Teleport, BloodHound, Consul</td><td>Completed</td></tr>
<tr><td><b><a href="./M3-Intelligence-and-SOAR/README.md">M3</a></b></td><td>Intelligence and SOAR</td><td>32 GB</td><td>Wazuh, Kafka, Logstash, OpenSearch, MISP, TheHive, Cortex, Shuffle, IntelOwl, SpiderFoot</td><td>Completed</td></tr>
<tr><td><b><a href="./M4-Deception-and-Honeypots/README.md">M4</a></b></td><td>Deception and Honeypots</td><td>16 GB</td><td>T-Pot, OpenCanary, Cowrie, Dionaea, Canarytokens, Tor honeypot</td><td>Completed</td></tr>
<tr><td><b><a href="./M5-Endpoint-Protection/README.md">M5</a></b></td><td>Endpoint Protection and Hardening</td><td>16 GB</td><td>Kali Linux (red team), Windows and Linux endpoints, Wazuh agent, Auditd, 802.1X, Group Policy, Teleport SSH</td><td>In progress</td></tr>
</table>

<br>

## Architecture and Integration

M1 is the hub. Every other machine depends on it for connectivity, DNS and perimeter enforcement.

### How the machines work together

<table>
<tr><th>From</th><th>To</th><th>What flows</th></tr>
<tr><td><b>M1 Network</b></td><td>M3</td><td>FortiGate syslog, Suricata alerts, Zeek logs and Arkime captures for correlation</td></tr>
<tr><td><b>M2 Identity</b></td><td>M1</td><td>FreeRADIUS for 802.1X, TACACS+ for FortiGate admin access, certificates from Step-CA</td></tr>
<tr><td><b>M2 Identity</b></td><td>M3</td><td>Keycloak, FreeRADIUS, Kerberos, Teleport and Vault logs through syslog and Kafka</td></tr>
<tr><td><b>M3 Intelligence</b></td><td>M1</td><td>Shuffle SOAR blocks IPs on FortiGate and domains on Pi-hole</td></tr>
<tr><td><b>M4 Deception</b></td><td>M3</td><td>Honeypot telemetry through Wazuh agents, Kafka and OpenSearch, which triggers SOAR</td></tr>
<tr><td><b>M5 Endpoint</b></td><td>M2, M3</td><td>Domain join, Group Policy and certificates from M2, endpoint telemetry to the M3 SIEM</td></tr>
</table>

### Zero Trust principles in this lab

<br>

## Key Results

<table>
<tr><th>Machine</th><th>Result</th></tr>
<tr><td><b>M1</b></td><td>Zone-based segmentation (WAN, DMZ, internal) enforced by FortiGate, with an IPS profile of 2,533 signatures and DMZ services behind Traefik and Squid</td></tr>
<tr><td><b>M2</b></td><td>Connectivity test passed for all 11 service endpoints; Consul service mesh with 5 of 5 nodes healthy</td></tr>
<tr><td><b>M3</b></td><td>48 Kafka topics, 3 Logstash pipelines, 25 custom Wazuh rules and 20 decoders, and 4 automated SOAR workflows with a reported alert-to-block time under 2 seconds</td></tr>
<tr><td><b>M4</b></td><td>4 honeypot VMs running 15+ honeypot tools, including a Tor hidden service, with all Wazuh agents active</td></tr>
<tr><td><b>M5</b></td><td>In progress</td></tr>
</table>

<br>

## Technology Stack

<table>
<tr><th>Plane</th><th>Technologies</th></tr>
<tr><td><b>Network (M1)</b></td><td><code>FortiGate</code> <code>VyOS</code> <code>pfSense</code> <code>Suricata</code> <code>Zeek</code> <code>RITA</code> <code>Arkime</code> <code>Pi-hole</code> <code>Traefik</code> <code>Squid</code> <code>NGINX</code> <code>MailCow</code></td></tr>
<tr><td><b>Identity (M2)</b></td><td><code>Samba AD</code> <code>Keycloak</code> <code>OpenLDAP</code> <code>FreeRADIUS</code> <code>Kerberos</code> <code>Vault</code> <code>Step-CA</code> <code>EJBCA</code> <code>Conjur</code> <code>Teleport</code> <code>BloodHound</code> <code>Consul</code></td></tr>
<tr><td><b>Intelligence (M3)</b></td><td><code>Wazuh</code> <code>Kafka</code> <code>Logstash</code> <code>OpenSearch</code> <code>Grafana</code> <code>Kibana</code> <code>Prometheus</code> <code>MISP</code> <code>OpenCTI</code> <code>TheHive</code> <code>Cortex</code> <code>Shuffle</code> <code>IntelOwl</code> <code>SpiderFoot</code></td></tr>
<tr><td><b>Deception (M4)</b></td><td><code>T-Pot</code> <code>OpenCanary</code> <code>Cowrie</code> <code>Dionaea</code> <code>Heralding</code> <code>Rdpy</code> <code>Mailoney</code> <code>ConPot</code> <code>Canarytokens</code> <code>Tor Honeypot</code></td></tr>
<tr><td><b>Endpoint (M5)</b></td><td><code>Kali Linux</code> <code>Wazuh Agent</code> <code>Auditd</code> <code>802.1X</code> <code>Group Policy</code> <code>Teleport SSH</code></td></tr>
</table>

<br>

## Documentation

<table>
<tr><th>Item</th><th>Description</th></tr>
<tr><td><b><a href="./docs/Zero-Trust-UTM-Report.pdf">Project report (PDF)</a></b></td><td>Full documentation of all five machines with configuration evidence</td></tr>
<tr><td><b><a href="./docs/Zero-Trust-UTM-Presentation.pptx">Presentation (PPTX)</a></b></td><td>70-slide walkthrough of the architecture and every machine</td></tr>
<tr><td><b>Demonstration videos</b></td><td>To be added in <code>docs/videos/</code></td></tr>
</table>

<br>

## Repository Structure

```
zero-trust-utm-security-lab/
├── README.md
├── LICENSE
├── docs/
│   ├── Zero-Trust-UTM-Report.pdf
│   ├── Zero-Trust-UTM-Presentation.pptx
│   └── videos/ (to be added)
├── M1-Core-Network-and-Security/
│   ├── README.md
│   └── images/
├── M2-Identity-and-PKI/
│   ├── README.md
│   └── images/
├── M3-Intelligence-and-SOAR/
│   ├── README.md
│   └── images/
├── M4-Deception-and-Honeypots/
│   ├── README.md
│   └── images/
└── M5-Endpoint-Protection/
    ├── README.md
    └── images/
```

<br>

## Project Team

<table>
<tr><th>Role</th><th>Name</th></tr>
<tr><td><b>Team</b></td><td>Abdul Wahid · Abdul Rafay Aziz · Usman · Hafsa</td></tr>
<tr><td><b>Supervisors</b></td><td>Professor Muhammad Azam · Professor Naveed Naeem</td></tr>
</table>

<br>

## License

**© 2026 Abdul Wahid, Abdul Rafay Aziz, Usman and Hafsa**. Licensed under [**CC BY-NC-ND 4.0**](./LICENSE) (Attribution, NonCommercial, NoDerivatives). You may view, reference and share a link to this work with credit. Commercial use and redistributing modified copies are not permitted.

<br>

## Disclaimer

This project was built in a private, isolated lab for educational purposes. No production systems or third-party targets were involved. All IP addresses, hostnames and credentials shown are lab-only values with no validity outside this environment.

<br>

## Contact

<p align="center">
  <b>Abdul Wahid</b>
</p>

<p align="center">
  <a href="mailto:a.wahid7860668@gmail.com"><img src="https://img.shields.io/badge/EMAIL-0d1117?style=for-the-badge&logo=gmail&logoColor=white&color=00b4d8" alt="Email"/></a>
  <a href="https://www.linkedin.com/in/abdul-wahid022"><img src="https://img.shields.io/badge/LINKEDIN-0d1117?style=for-the-badge&logo=linkedin&logoColor=white&color=00b4d8" alt="LinkedIn"/></a>
  <a href="https://github.com/abdul-wahid022"><img src="https://img.shields.io/badge/GITHUB-0d1117?style=for-the-badge&logo=github&logoColor=white&color=00b4d8" alt="GitHub"/></a>
</p>
