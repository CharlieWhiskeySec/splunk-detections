<p align="center">
  <img src="https://github.com/user-attachments/assets/02a7bd81-ee10-4cf1-aef9-c0649a03a4ea" width="200">
</p>

<h1 align="center">
Splunk Detection Engineering
</h1>

## Overview

This repository contains detection logic, dashboards, and engineering notes developed within a self-built Active Directory security environment using Splunk Enterprise.

The focus is not simply writing SPL searches, but understanding how Windows security telemetry is ingested, interpreted, and transformed into actionable security detections. Each detection is developed using real events generated within the lab environment and includes supporting engineering decisions, troubleshooting steps, and investigation guidance.

## Objectives

- Develop practical Splunk detections using Windows Security Events.
- Build reusable SPL for authentication and identity monitoring.
- Document engineering challenges encountered during development.
- Map detections to real-world security use cases.
- Demonstrate security monitoring and detection engineering skills through hands-on projects.

## Detection Categories

### Authentication Monitoring

- Successful Logons — Event ID 4624
- Failed Logons — Event ID 4625
- Account Lockouts — Event ID 4740
- Privileged Logons — Event ID 4672

### Account Management

- User Creation
- User Deletion
- Security Group Membership Changes

### Process Monitoring

- Windows Process Creation
- PowerShell Activity
- Sysmon Process Telemetry

## Engineering Philosophy

Every detection in this repository is developed using telemetry generated within the lab environment rather than sample data. When default field extractions are insufficient, detections are refined using additional parsing techniques such as field extraction and regular expressions.

Engineering decisions, troubleshooting steps, and validation notes are documented alongside each detection to demonstrate the complete development process.

## Current Work

The first detection being developed focuses on Windows account lockouts using Event ID 4740. During testing, the event was successfully ingested into Splunk, but the default field extractions did not expose the locked account or caller computer in a usable table format.

To solve this, the raw event was inspected and custom `rex` field extractions were used to extract the required values.

## Detection Template

Each detection will follow a consistent format:

```markdown
# Detection Name

## Objective

## Security Use Case

## Data Source

## SPL

## Expected Output

## Engineering Challenges

## Validation

## MITRE ATT&CK Mapping

## Investigation Guidance

## Future Improvements

```

### Technologies

- Splunk Enterprise
- Windows Server
- Active Directory
- Windows Security Event Logs
- Sysmon

### Skills

- Detection Engineering
- SPL Search Development
- Windows Event Analysis
- Field Extraction
- Authentication Monitoring
- Security Troubleshooting
- Technical Documentation

- ## Documentation

- [Engineering Challenges](engineering-challenges.md)
- [Windows Security Event Reference](windows-event-reference.md)
- [Event ID 4740 - Account Lockout Detection](authentication/4740-account-lockout-detection.md)

- ## Related Projects

- [Active Directory Security Lab](https://github.com/CharlieWhiskeySec/active-directory-security-lab)
- [SOC Investigations](https://github.com/CharlieWhiskeySec/soc-investigations)
- [Traffic Analysis](https://github.com/CharlieWhiskeySec/traffic-analysis)
