---
layout: article
title: "Config Sentinel — Unsaved Configuration Drift Detection"
mode: immersive
header:
  theme: dark
article_header:
  type: overlay
  theme: dark
  background_color: '#1a5276'
  background_image:
    gradient: 'linear-gradient(135deg, rgba(26,82,118, .7) 0%, rgba(46,134,193, .6) 50%, rgba(133,193,233, .5) 100%)'
sharing: true
comment: true
mathjax: false
mathjax_autoNumber: false
tags: NetBrain Cisco IOS automation configuration-management network-intent
---

A NetBrain Network Intent solution for automatically detecting configuration drift between running and startup configurations on Cisco IOS/IOS-XE devices.

<!--more-->

### The Problem This Solves

- A device reloads after a power event. The running config had 3 weeks of undocumented, unsaved changes. Everything is gone.
- A change control was applied but the engineer was paged mid-task and never ran `write mem`. The audit shows clean.
- A network team inherits an environment with unknown drift across 200 devices. There is no fast way to assess.

### What Config Sentinel Delivers

- **Automated per-device drift detection at scale** — across hundreds of devices in a single run.
- **Clear diff output** showing exactly what lines are in running config but absent from startup.
- **Green / Red per-device health status** surfaced directly in the NetBrain map.
- **Zero manual SSH required** — fully integrated into the NetBrain intent workflow.

---

## 2. How It Works — Architecture Overview

Config Sentinel is built as a two-step NetBrain Network Intent: a data retrieval command block followed by a diagnosis logic block. The two components are wired together via parsed variables that carry configuration data from device output into the diagnostic engine.

### 2.1 High-Level Flow

```
1 Device Poll → 2 Command Execute → 3 Regex Parse → 4 Variable Extract → 5 Logic Evaluate → 6 Diagnosis Message → 7 Map Status
```

### 2.2 Component Breakdown

The intent is composed of two numbered command entries in the NetBrain command canvas:

| Step | Component | Purpose |
|------|-----------|---------|
| 1 | **Switch — show archive config differences** | Executes the IOS command that outputs the diff between running and startup config. This command is native to IOS/IOS-XE and requires no additional configuration on the device. |
| 1.1 | **Parser: Format1 (Baseline)** | Applies a regex-based variable extraction pattern to the raw command output. Pulls out four key variables: `$whole_output_header`, `$config_differences_full`, `$diff_header`, and `$diff_content`. |
| 1.2 | **Diagnosis 1** | Wires the parsed variables into the diagnostic logic engine. All conditional logic, variable transformations, and status messages are defined here. |
| 2 | **Intent — \<Intent\>** | The second command slot is reserved for the Intent-level logic block, used to set Global Variables and carry information across devices or sub-intents if needed. |
| 2.1 | **Global Variables (1)** | Stores the final `result` variable at intent scope, making it available for reporting or cross-device intent logic. |


---

The dynamic table provides a tabular summary of all devices in scope, with pass/fail status for each device at a glance.

![NetBrain dynamic table showing per-device Config Sentinel results]({{ site.baseurl }}/assets/images/sentinel/dynamic_table.png)

Results can also be exported to Excel for reporting, audit evidence, or offline review. Devices with unsaved changes are highlighted in red with the full diff content in the status column.

![Excel export of Config Sentinel results with green/red device status and diff details]({{ site.baseurl }}/assets/images/sentinel/excel_export.png)

---

## 5. Business Use Cases

Config Sentinel addresses a range of real-world operational scenarios across network teams of all sizes:

#### Post-Change Control Verification

#### Post-Outage / Post-Reboot Audit

#### Estate Drift Discovery (Day 1 Assessment)

#### Unauthorized Change Detection

#### Scheduled Hygiene Check


---

## 6. Design Decisions & Lessons Learned

| Decision | Rationale |
|----------|-----------|
| **Why `show archive config differences` vs manual diff?** | This IOS-native command is the cleanest, most reliable source of truth for config drift. It leverages the IOS archive subsystem and does not require any additional tooling or feature configuration on the device. |
| **Why double-condition the ElseIf (A and B)?** | Testing only for "does not contain `!No changes were found`" could produce false positives if the command fails or returns unexpected output. The second condition (`$diff_content` is not empty) ensures we only flag devices where we have actual diff content to display. |
| **Why use ReplacebyRegex before Trim?** | Raw IOS output often contains runs of `\n\n` characters between config blocks. Without collapsing these first, the Trim function would not clean the output fully. The sequence matters: normalize first, then trim edges. |
| **Why set Status Code to Device (not Intent) in the ElseIf branch?** | The intent should flag individual devices, not the entire run. If 198 out of 200 devices are clean, the intent-level status would be misleading if set to red. Device-level status ensures the map accurately reflects which specific nodes need attention. |
| **Why use Intent Template mode?** | Intent Templates are shareable across customer environments and can be applied to device groups. This makes the intent reusable across Cisco IOS/IOS-XE devices in any project without rebuild. |

---

## 7. Extensibility & Future Enhancements

Config Sentinel is intentionally scoped to Cisco IOS/IOS-XE in v1.0. The following extensions are planned or recommended:

| Enhancement | Description |
|-------------|-------------|
| **Multi-vendor support** | Extend to Aruba/HP (`display saved-configuration`), Palo Alto (`show config diff`), Juniper (`show | compare rollback`), and NX-OS using vendor-specific command blocks within the same intent template. |
| **Line count metric** | Add a variable that counts the number of unsaved lines (`$diff_content`). Surface this as a numeric metric in the diagnosis message to help prioritize remediation effort by severity. |
| **Scheduled Automation** | Wire the intent into a NetBrain automation schedule. Run nightly across all production devices and push results to a SIEM or ticketing system via NetBrain Automation API. |
| **Save to Incident** | Enable the "Save to Incident" checkbox in the Diagnosis Message block. This allows NetBrain to automatically attach the diff output to incident tickets for devices that are flagged. |
| **Remediation Runbook Link** | Add a hyperlink to the diagnosis message pointing to the internal wiki page for the `write memory` procedure, giving NOC staff a direct path to resolution from within the intent result panel. |

---

## Summary

Config Sentinel provides a lightweight, zero-dependency solution to one of the most common and costly operational oversights in network management: unsaved configuration changes.

Built entirely within the NetBrain intent framework — no external scripts, no third-party tooling, no additional infrastructure — it gives any engineering team instant visibility into config drift across their Cisco estate.

The build time for a fully functional intent is under 30 minutes. The operational value is immediate.

---

| | |
|---|---|
| **Document Type** | Solution Design & Build Guide |
| **Author** | Alejandro Marin — Professional Services Engineer |
| **Platform** | NetBrain Technologies (Network Intent Framework) |
| **Target Devices** | Cisco IOS / IOS-XE (Switch, Router) |
| **Version** | 1.0 |
| **Date** | March 2026 |
