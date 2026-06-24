# Event ID 4740 - Account Lockout Detection

**Status:** 🚧 In Progress

**Severity:** Medium

**Data Source:** Windows Security Log

**Platform:** Splunk Enterprise

**Last Updated:** June 2026

---

## Objective

Detect Windows account lockout events (Event ID 4740) to identify users who have exceeded the configured account lockout threshold. This detection helps identify potential brute-force attacks, password spraying, user error, or other authentication-related issues.

---

## Security Use Case

Account lockout events may indicate repeated authentication failures caused by user error or malicious activity. Monitoring these events provides visibility into authentication anomalies and can help security teams quickly identify compromised accounts or password attack attempts.

---

## Data Source

- Windows Security Log
- Event ID 4740
- Splunk Enterprise
- Active Directory Domain Controller

---

## SPL Query

*To be added during implementation.*

---

## Expected Output

*To be documented after validation.*

---

## Validation

*To be completed after generating and validating Event ID 4740 within the lab environment.*

---

## Engineering Challenges

See **engineering-challenges.md**

- Challenge 001 – Event ID 4740 Field Extraction
- Challenge 002 – Timestamp Validation
- Challenge 003 – Account Lockout Policy Validation

---

## MITRE ATT&CK Mapping

*To be added.*

---

## Investigation Guidance

*To be developed.*

---

## Future Improvements

- Correlate with Event ID 4625 (Failed Logon)
- Build an authentication timeline
- Add dashboard visualization
- Reduce false positives through threshold tuning
- Correlate lockout events with source workstation activity
