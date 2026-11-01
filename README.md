# Wazuh SIEM Home Lab

## Project Overview
Deployed a fully functional Wazuh SIEM (Security Information and Event Management) home lab on a MacBook Air (Apple Silicon) using UTM for virtualization. The goal was to centralize log collection and implement custom detection engineering.

## Architecture
- **Manager Node:** Ubuntu Server (Intel x86_64 emulated via QEMU), running Wazuh 4.9.2 via Docker.
- **Endpoint Node:** Ubuntu Desktop (ARM64 native), running Wazuh Agent 4.9.2.
- **Networking:** UTM Port Forwarding (Host `127.0.0.1:9443` -> Guest `10.0.2.15:443`).

## Detection Engineering
Wrote a custom correlation rule to detect SSH brute-force attempts by monitoring built-in authentication failure logs (Rule ID 5760).

### Custom Rule (`local_rules.xml`)
```xml
<rule id="100101" level="10">
  <if_sid>5760</if_sid>
  <description>SSH Brute Force Attempt Detected (Custom Correlation)</description>
  <group>ssh,authentication_failed,custom</group>
</rule>

Challenges & Troubleshooting
Cross-Architecture Emulation: Successfully navigated the challenges of running x86_64 Wazuh Docker containers on an ARM64 Mac by utilizing QEMU user-static emulation and tonistiigi/binfmt.

Docker Formatting: Resolved critical YAML configuration errors (bad indentation of a mapping entry) that were preventing the Wazuh Dashboard from booting.

Network Routing: Configured UTM virtual network interfaces to ensure seamless log ingestion between the Endpoint and Manager.


