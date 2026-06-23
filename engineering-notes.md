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
