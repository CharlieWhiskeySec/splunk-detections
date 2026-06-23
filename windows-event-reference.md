# Windows Security Event Reference

This document tracks the Windows Security Event IDs monitored within this project and serves as a roadmap for future detection engineering work.

Each event will eventually have its own documented detection, screenshots, investigation guidance, and engineering notes.

| Event ID | Description | Status |
|----------|-------------|--------|
| 4624 | Successful Logon | Planned |
| 4625 | Failed Logon | Planned |
| 4648 | Logon Using Explicit Credentials | Planned |
| 4672 | Special Privileges Assigned | Planned |
| 4688 | Process Creation | Planned |
| 4720 | User Created | Planned |
| 4726 | User Deleted | Planned |
| 4732 | User Added to Security Group | Planned |
| 4733 | User Removed from Security Group | Planned |
| 4740 | Account Lockout | In Progress |
| 4768 | Kerberos Authentication Ticket (TGT) | Planned |
| 4769 | Kerberos Service Ticket | Planned |
| 4771 | Kerberos Pre-Authentication Failed | Planned |
| 4799 | Security Group Enumeration | Planned |
| 4907 | Auditing Settings Changed | Planned |

## Development Workflow

Each detection developed in this repository will include:

- Objective
- Security Use Case
- Data Source
- SPL Query
- Expected Output
- Engineering Challenges
- Validation
- MITRE ATT&CK Mapping
- Investigation Guidance
- Future Improvements
