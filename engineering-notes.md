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

Investigation determined that event ingestion was functioning correctly and that the issue was not related to the Splunk Universal Forwarder or Windows event collection.

The lab environment was using the Splunk Free license, which does not provide access to user preferences for adjusting the default timezone. As a result, relative time searches did not align with the timestamps being displayed within the environment.

To continue development without interrupting progress, event validation was performed using All Time searches while documenting the platform limitation for future reference.

## Lessons Learned

Not every issue is caused by incorrect configuration or data ingestion. Platform limitations can also influence how data is presented and queried.

Before troubleshooting log sources, verify whether the behavior is related to licensing restrictions, timezone settings, or search time ranges. Understanding the capabilities and limitations of the platform is just as important as understanding the data itself.

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

