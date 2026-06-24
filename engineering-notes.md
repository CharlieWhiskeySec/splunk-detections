# Engineering Challenges

This document captures technical challenges encountered while developing detections and integrating Windows telemetry into Splunk Enterprise.

The goal is to document not only the final solution, but also the engineering process used to troubleshoot, validate, and improve each detection.

---

# Challenge 001 - Event ID 4740 Field Extraction

## Problem

Windows Security Event ID 4740 (Account Lockout) was successfully ingested into Splunk. However, the default field extractions did not expose the locked account name or originating workstation as searchable fields.

## Investigation

The event was confirmed to exist within the Windows Security log.

Inspection of the raw event showed that both values were present, but were not automatically extracted into searchable fields.

Rather than assuming the event source or data ingestion process was incorrect, the raw event structure was reviewed to determine how the missing values could be extracted.

## Solution

Custom `rex` expressions were developed to extract the locked account and caller computer directly from the raw event.

This allowed the detection to produce clean, usable output without modifying the underlying log source.

## Lessons Learned

Always inspect the raw event before troubleshooting data collection or ingestion.

Many Windows Security events contain valuable information that may require custom field extraction depending on the event structure, data source, or Splunk environment.

---

# Challenge 002 - Timestamp Validation

## Problem

Windows Security events were successfully reaching Splunk, but relative time searches such as **Last 15 Minutes** and **Last 30 Minutes** returned no results, even though recent events had been generated.

## Investigation

Initial troubleshooting focused on verifying that the Splunk Universal Forwarder was still sending events and that Windows Security logs were continuing to update.

Further investigation confirmed that the events were present when using an **All Time** search, indicating that data ingestion was functioning correctly.

The issue was determined to be related to timestamp interpretation within the lab environment rather than a failure to collect or index events.

## Solution

Event validation was performed using **All Time** searches while the timestamp discrepancy was investigated. This allowed detection development to continue without interrupting progress on the project.

## Lessons Learned

Before assuming data collection has failed, verify whether the issue is related to event timestamps, timezone configuration, or search time ranges.

Validating event ingestion using broader search windows can quickly distinguish between an ingestion problem and a timestamp-related issue.

---

# Challenge 003 - Account Lockout Policy Validation

## Problem

Repeated failed authentication attempts did not generate Windows Security Event ID 4740 (Account Lockout), preventing development of an account lockout detection.

## Investigation

Initial troubleshooting focused on confirming that failed logon events were being generated correctly.

After verifying that repeated authentication failures were occurring without producing a lockout event, Active Directory account lockout settings were reviewed.

The Default Domain Policy revealed that the account lockout threshold was configured to **0**, meaning accounts would never lock regardless of the number of failed authentication attempts.

## Solution

An Account Lockout Policy was implemented within Group Policy by configuring a lockout threshold, lockout duration, and reset counter interval.

After applying the policy, repeated failed logon attempts successfully generated Event ID 4740, allowing account lockout detections to be developed and validated in Splunk.

## Lessons Learned

When expected Windows Security Events are not generated, verify that the underlying Windows security policy actually produces the event.

Detection engineering depends not only on collecting logs, but also on ensuring the operating system is configured to generate the desired telemetry.

