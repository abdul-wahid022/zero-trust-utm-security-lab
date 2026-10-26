<div align="center">

<img src="./images/cover.png" alt="M3 — Intelligence and SOAR Plane" width="100%" />

<br><br>

<h1>Machine 3 — Intelligence and SOAR Plane</h1>
<h3>SIEM, Threat Intelligence and Automated Response</h3>

<br>

<img src="https://img.shields.io/badge/MACHINE-3%20%2F%205-0d1117?style=for-the-badge&labelColor=0d1117&color=00b4d8" />
<img src="https://img.shields.io/badge/STATUS-COMPLETED-0d1117?style=for-the-badge&labelColor=0d1117&color=00b4d8" />
<img src="https://img.shields.io/badge/RAM-32%20GB-0d1117?style=for-the-badge&labelColor=0d1117&color=00b4d8" />
<img src="https://img.shields.io/badge/VMS-3-0d1117?style=for-the-badge&labelColor=0d1117&color=00b4d8" />
<img src="https://img.shields.io/badge/SOAR%20WORKFLOWS-4-0d1117?style=for-the-badge&labelColor=0d1117&color=00b4d8" />

<br><br>

<a href="#architecture"><b>Architecture</b></a> &nbsp;·&nbsp;
<a href="#log-pipeline"><b>Pipeline</b></a> &nbsp;·&nbsp;
<a href="#cti-m3-tools"><b>CTI Tools</b></a> &nbsp;·&nbsp;
<a href="#soar-workflows"><b>SOAR</b></a> &nbsp;·&nbsp;
<a href="#integration-with-other-machines"><b>Integration</b></a>

</div>

<br>

---

## Executive Summary

Machine 3 is the intelligence and SOAR plane of the Zero Trust UTM project, the brain of the lab. It collects logs from every machine, enriches them with threat intelligence, detects threats in real time and orchestrates automated responses. M1 enforces network policy and M2 manages identity, while M3 makes the system self-aware and self-defending.

M3 has the highest RAM allocation in the cluster (32 GB of 104 GB) because it runs the heaviest stack: OpenSearch, Kafka with 48 topics, Logstash pipelines, Wazuh, MISP, TheHive, Cortex, IntelOwl, Shuffle and a local LLM-based dark web tool.

> All testing was performed in a private, isolated lab. All IP addresses shown are lab-only values.

<br>

## Architecture

<p align="center">
  <img src="./images/architecture.png" alt="M3 intelligence plane architecture" width="800"/><br/>
  <sub>M3 intelligence plane: three VMs and their data flows</sub>
</p>

<table>
<tr><th>VM</th><th>IP Address</th><th>Role</th><th>Components</th></tr>
<tr><td><b>SIEM-M3</b></td><td><code>192.168.35.133</code></td><td>Log collection and event management</td><td>Wazuh Manager 4.14.5, Kafka, Logstash, OpenSearch</td></tr>
<tr><td><b>MONITOR-M3</b></td><td><code>192.168.35.136</code></td><td>Visualization and monitoring</td><td>Wazuh Dashboard, Grafana, Kibana, Prometheus</td></tr>
<tr><td><b>CTI-M3</b></td><td><code>192.168.35.134</code></td><td>Threat intelligence and SOAR</td><td>MISP, OpenCTI, TheHive, Cortex, Shuffle, IntelOwl, SpiderFoot, Dark Web CTI tool</td></tr>
</table>

<br>

## Log Pipeline

<p align="center">
  <img src="./images/siem-stack.png" alt="SIEM stack" width="800"/><br/>
  <sub>Wazuh, Kafka, Logstash, OpenSearch and Grafana</sub>
</p>

<table>
<tr><th>Stage</th><th>What happens</th></tr>
<tr><td><b>1. Collect</b></td><td>Wazuh agents, FortiGate syslog, Suricata, Zeek, honeypots and application logs</td></tr>
<tr><td><b>2. Ingest</b></td><td>Apache Kafka streams logs through 48 topics</td></tr>
<tr><td><b>3. Process</b></td><td>Logstash parses and tags events through 3 pipelines and adds GeoIP data</td></tr>
<tr><td><b>4. Store</b></td><td>OpenSearch indexes the enriched logs</td></tr>
<tr><td><b>5. Visualize</b></td><td>Wazuh Dashboard, Kibana and Grafana on MONITOR-M3</td></tr>
<tr><td><b>6. Detect</b></td><td>25 custom Wazuh rules and 20 custom decoders; level 10+ alerts trigger SOAR, level 12+ create TheHive cases</td></tr>
</table>

<p align="center">
  <img src="./images/wazuh-siem.png" alt="Wazuh SIEM" width="800"/><br/>
  <sub>Wazuh custom rules and agent deployment</sub>
</p>

<p align="center">
  <img src="./images/kafka-logstash.png" alt="Kafka topics and Logstash pipelines" width="800"/><br/>
  <sub>Kafka topics and Logstash pipelines</sub>
</p>

<br>

## CTI-M3 Tools

<p align="center">
  <img src="./images/cti-stack.png" alt="CTI-M3 stack" width="800"/>
</p>

<table>
<tr><th>Tool</th><th>Role</th><th>Detail</th></tr>
<tr><td><b>MISP</b> 2.5.37</td><td>Threat intelligence sharing</td><td>Central IOC repository with automated feeds such as CIRCL, Botvrij, Emerging Threats and Tor exit nodes</td></tr>
<tr><td><b>IntelOwl</b> 6.6.1</td><td>Observable enrichment</td><td>80 analyzers and 6 connectors, including MISP and OpenCTI</td></tr>
<tr><td><b>SpiderFoot</b></td><td>Automated OSINT</td><td>Target analysis with four scan modes</td></tr>
<tr><td><b>TheHive</b> 4.0.0-1</td><td>Case management</td><td>Alerts and cases created from MISP, with MITRE ATT&amp;CK tagging</td></tr>
<tr><td><b>Cortex</b> 4.0.0-1</td><td>Analysis engine</td><td>Analyzers such as AbuseIPDB, Shodan, VirusTotal, MaxMind and URLscan</td></tr>
<tr><td><b>Shuffle</b></td><td>SOAR</td><td>Webhook-triggered playbooks</td></tr>
<tr><td><b>OpenCTI</b></td><td>Knowledge graph</td><td>STIX2 view of threat actor campaigns</td></tr>
<tr><td><b>Dark Web CTI tool</b></td><td>Dark web OSINT</td><td>Custom tool using Llama-3.3-70b through a local Ollama instance with 4 scraping threads</td></tr>
</table>

<p align="center">
  <img src="./images/misp.png" alt="MISP feeds and events" width="800"/><br/>
  <sub>MISP threat intelligence feeds and events</sub>
</p>

<p align="center">
  <img src="./images/intelowl-dashboard.png" alt="IntelOwl dashboard" width="800"/><br/>
  <sub>IntelOwl dashboard and connectors</sub>
</p>

<p align="center">
  <img src="./images/intelowl-live-scan.png" alt="IntelOwl live scan" width="800"/><br/>
  <sub>IntelOwl live scan and analysis</sub>
</p>

<p align="center">
  <img src="./images/spiderfoot.png" alt="SpiderFoot" width="800"/>
</p>

<p align="center">
  <img src="./images/thehive-case-management.png" alt="TheHive case management" width="800"/><br/>
  <sub>TheHive case management with MISP integration</sub>
</p>

<p align="center">
  <img src="./images/thehive-alerts.png" alt="TheHive alerts" width="800"/><br/>
  <sub>TheHive alerts and cases</sub>
</p>

<p align="center">
  <img src="./images/cortex-setup.png" alt="Cortex analyzer setup" width="800"/><br/>
  <sub>Cortex organization, users and TheHive integration</sub>
</p>

### Dark Web CTI tool

<p align="center">
  <img src="./images/darkweb-cti-tool.png" alt="Dark Web CTI tool" width="800"/>
</p>

<p align="center">
  <img src="./images/darkweb-analysis.png" alt="Dark web analysis" width="800"/><br/>
  <sub>Analysis of a WannaCry ransomware search</sub>
</p>

### Threat intelligence pipeline

<p align="center">
  <img src="./images/threat-intel-pipeline.png" alt="Threat intelligence pipeline" width="800"/>
</p>

<br>

## SOAR Workflows

Shuffle SOAR runs four playbooks. The report states an alert-to-action time of under 2 seconds. The FortiGate block workflow editor screenshot still shows two open workflow issues, so that workflow is documented but not fully verified.

<p align="center">
  <img src="./images/shuffle-soar.png" alt="Shuffle SOAR" width="800"/>
</p>

<p align="center">
  <img src="./images/shuffle-workflows-summary.png" alt="Shuffle workflows summary" width="800"/>
</p>

<table>
<tr><th>#</th><th>Workflow</th><th>Trigger</th><th>Action</th></tr>
<tr><td><b>1</b></td><td>Wazuh critical alert → FortiGate block</td><td>Wazuh alert level 10 or above</td><td>Block the source IP through the FortiGate API</td></tr>
<tr><td><b>2</b></td><td>Critical alert → TheHive case</td><td>High-severity Wazuh alert (level 12 or above)</td><td>Create a TheHive case, enriched through Cortex (AbuseIPDB and GeoIP)</td></tr>
<tr><td><b>3</b></td><td>Honeypot alert → multi-layer block</td><td>Interaction with an M4 honeypot</td><td>Block on FortiGate and OPNsense, add the IOC to MISP and create a TheHive case</td></tr>
<tr><td><b>4</b></td><td>MISP IOC → Pi-hole block</td><td>New domain or URL IOC in MISP</td><td>Push the domain to the Pi-hole blocklist</td></tr>
</table>

<p align="center">
  <img src="./images/shuffle-workflow-editor.png" alt="Shuffle workflow editor" width="800"/>
</p>

<p align="center">
  <img src="./images/shuffle-playbook-flow.png" alt="Shuffle playbook flow" width="800"/>
</p>

<br>

## Integration with Other Machines

<table>
<tr><th>Machine</th><th>Integration</th></tr>
<tr><td><b>M1 Network</b></td><td>Receives Suricata, Zeek and firewall logs; SOAR sends block commands to FortiGate and Pi-hole</td></tr>
<tr><td><b>M2 Identity</b></td><td>Receives Keycloak, Vault, RADIUS and certificate logs through Kafka</td></tr>
<tr><td><b>M4 Deception</b></td><td>Receives honeypot logs from four Wazuh agents over Tailscale and triggers the multi-layer block workflow</td></tr>
<tr><td><b>M5 Endpoint</b></td><td>Receives endpoint telemetry: Wazuh agent logs, Sysmon, Auditd, OSQuery and Falco events</td></tr>
</table>

<br>

## Lab Environment

| | |
|---|---|
| **Platform** | 3 VMs on M3 (32 GB RAM) |
| **Wazuh agents** | 5 active (CTI-M3 and the four M4 honeypots) |
| **Scope** | Isolated lab only |

<br>

## Full Report

📄 **[Zero-Trust-UTM-Report.pdf](../docs/Zero-Trust-UTM-Report.pdf)**

<br>

<div align="center">

⬅ <a href="../"><b>Back to Main Repository</b></a>

</div>
