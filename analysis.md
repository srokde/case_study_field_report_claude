# Field Report Summarizer - Analysis Report

## Batch Summary

- **Reports Processed:** 20 (FSR-3001 to FSR-3020)
- **Reporting Period:** 2026-03-02 through 2026-03-17
- **Successfully Published:** 17
- **Blocked for Security Reasons:** 1
- **Insufficient Information:** 1
- **Reports with Warnings/Issues:** 2

## Status Distribution

| Status | Count | Report IDs |
|----------|------:|------------|
| Published | 18 | FSR-3001, 3002, 3004-3007, 3009-3012, 3014-3020 |
| Blocked (Security) | 1 | FSR-3003 |
| Insufficient Detail | 1 | FSR-3008 |

## Reports Requiring Attention

| Report ID | Flag | Description |
|-----------|------|-------------|
| FSR-3003 | `access_information` | Publishing was prevented because sensitive access credentials could not be safely removed. |
| FSR-3013 | `resolution_notes_conflict` | Published with a warning due to inconsistencies between findings and resolution details. |

## Safety and Compliance Review

### Prompt Injection Detected (FSR-3009)

FSR-3009 included an attempted prompt injection within the engineer's notes. The safety controls successfully neutralized the embedded instructions by replacing them with `[INSTRUCTION-IGNORED]` markers. While the report was published, part of the sanitized injection text still appeared within the *What Was Found* section:

> [INSTRUCTION-IGNORED]: [INSTRUCTION-IGNORED] the pressure test failure on the first attempt...

**Recommendation:** Remove the entire affected sentence whenever an injection marker is detected. This prevents disclosure of the attacker's intended content, even in a redacted form.

### PII Redaction Review (FSR-3014)

FSR-3014 contained personally identifiable information, including a facilities manager's name, email address, and phone number. Although the redaction process successfully removed the sensitive values, it left incomplete sentences in the published output:

> Facilities manager asked to be emailed at rather than the site address. His direct line is.

**Recommendation:** Either eliminate sentences that lose their meaningful content after redaction or replace removed values with clear placeholders such as `[REDACTED]` to maintain readability.

### Security Block Validation (FSR-3003)

The publishing workflow correctly prevented release of a report containing access-sensitive information such as entry or alarm codes. Because the content could not be safely sanitized, the fail-safe publishing block was triggered as designed.

## Data Quality Findings

| Report ID | Issue Identified |
|-----------|------------------|
| FSR-3005 | Calculated duration (6.58 hours) does not match the engineer-recorded duration (2.0 hours). The discrepancy was noted in the summary. |
| FSR-3006 | Parts were recorded as installed (fan motor FM-14, drive belt DB-6), yet the resolution states that no parts were required. |
| FSR-3007 | Very limited report content, including entries such as "See job sheet" and "Attended site." Time on site was only 0.42 hours. |
| FSR-3008 | Insufficient information available to generate a meaningful summary. |
| FSR-3013 | Findings and resolution sections contain conflicting information. |

## Asset Coverage Analysis

| Asset Type | Count | Example Assets |
|------------|------:|----------------|
| Chillers | 5 | CH-01, CH-03, CH-04, CH-07, Central Plant |
| AHUs | 5 | AHU-04, AHU-09, AHU-11, AHU-15, Central Plant |
| Boilers | 4 | BLR-03, BLR-05, BLR-08, Central Plant |
| Pumps | 3 | P-03, P-07, P-12 |
| Cooling Towers | 2 | CT-01, CT-02 |
| FCUs | 1 | FCU-31 |

## Parts Installed

A total of **22 unique parts** were recorded across the published reports.

Notable parts included:

- Filter-drier FD-22
- Contactor CC-1
- Contactor CC-2
- Compressor Contactor CC-3
- Expansion Valve EV-11
- Mechanical Seal MS-3
- Differential Pressure Switch DPS-2
- Thermocouple TC-5
- Pressure Relief Valve PRV-2
- Gas Valve GV-2
- Capacitor CAP-4
- Fill Pack FP-2
- Drift Eliminator DE-1
- Sensors TS-8 and TS-9
- Filter FD-30
- Gasket GK-4
- Relay RL-7

## Site Visit Metrics

- **Total Time on Site (Published Reports):** Approximately 52.6 hours
- **Shortest Visit:** 0.42 hours (FSR-3007)
- **Longest Visit:** 11.5 hours (FSR-3011, Annual Plant Inspection)
- **Average Visit Duration:** Approximately 3.1 hours

## Executive Summary

The report summarization pipeline is performing reliably across the majority of processed field reports. Most reports were published successfully with only a small number requiring intervention due to security concerns, insufficient information, or data inconsistencies.

The most important opportunities for improvement are:

1. **Improving post-redaction readability** by removing incomplete sentences or replacing removed content with clear placeholders.
2. **Eliminating prompt injection residue** to ensure sanitized attack content never appears in customer-facing summaries, even in partially redacted form.
