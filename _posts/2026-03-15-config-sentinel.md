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

## 1. Executive Summary

Config Sentinel is a NetBrain Network Intent designed to automatically detect configuration drift between the running and startup configurations on Cisco IOS/IOS-XE devices.

When a network engineer makes changes to the running config and forgets to issue a `write memory` (or `copy running-config startup-config`), those changes exist only in volatile memory — one reload away from being permanently lost.

This intent continuously monitors for this gap across the network, exposing unsaved changes in clear, human-readable output. It eliminates the need for engineers to manually log into devices to check for drift and creates an auditable record of what changed — without requiring any additional tooling beyond NetBrain.

---

### The Problem This Solves

- A device reloads after a power event. The running config had 3 weeks of undocumented changes. Everything is gone.
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

## 3. Step-by-Step Build Guide

This section documents exactly how Config Sentinel was built inside NetBrain. Follow these steps to reproduce the intent from scratch in any NetBrain environment.

### Step 1 — Create the Network Intent

1. Navigate to **Shared Intents** in the NetBrain Intent Library.
2. Click **+ New Intent** and name it: `Unsaved Configs [startup vs running diff]`
3. Place the intent under the folder path: `Device Health / Unsaved Configs`
4. Enable the **Intent Template** toggle (top-right) so the intent can be shared across device groups.
5. Add a description (optional): *Detects configuration drift between running and startup config on Cisco IOS/IOS-XE devices by parsing the output of 'show archive config differences'.*

> **Naming Convention Note:** The bracket notation `[startup vs running diff]` is used to make the intent self-describing in the intent library. Engineers browsing the list immediately understand what data source the intent uses.

### Step 2 — Add Command Block (Row 1): show archive config differences

1. Click **+ Add Command** in the intent command canvas.
2. Set **Device Type** to: `switch` (Cisco IOS/IOS-XE)
3. Set **Command** to: `show archive config differences`

This command natively outputs the diff between the startup-config and running-config.

Example raw output from a device with unsaved changes:

```
switch#show archive config differences
!Contextual Config Diffs:
+interface g0/0
+shutdown
```

The `+` prefix indicates lines present in the running config that are absent from the startup config. This is the content we want to extract and evaluate.

> **When There Are No Unsaved Changes:** If the running config matches the startup config, the output contains the string `!No changes were found`. This is the key string the diagnostic logic tests for in the If (A) condition.

### Step 3 — Configure the Parser (Variable Extraction)

Click on the parser icon next to the command row to open the variable definition editor. This is where regex patterns are applied to extract structured data from the raw output.

#### 3.1 Parser Configuration: Format1 (Baseline)

The parser uses a single pattern called `diff_xpar` with Type: **Single**. The extraction is anchored to the contextual config diffs section of the output.

| Field | Value / Pattern | Description |
|-------|----------------|-------------|
| Pattern Name | `diff_xpar` | Named pattern used for the contextual diff section |
| Type | Single | Extracts one match from the full output |
| Start Anchor | `!Contextual Config` | Marks beginning of diff section — matches the IOS header line |
| End Anchor | *(empty)* | No end anchor; captures to end of output |
| Var Line 4 | `LinesByVariable` | `[$diff_content]:$diff_header-` Splits content by line using the `diff_header` as delimiter |
| Field 3 (line 3) | `+interface g0/0 → 1 Line` | Captures the first interface change line as a sample anchor |

#### 3.2 Output Variables Produced by the Parser

After parsing, the following four variables are available for use in the diagnostic block:

| Variable Name | Type | Example Value |
|--------------|------|---------------|
| `$whole_output_header` | string | `differences` |
| `$config_differences_full` | string | `!Contextual Config Diffs: +interface g0/0 +shutdown` |
| `$diff_header` | string | `Diffs:` |
| `$diff_content` | string | `+interface g0/0 +shutdown` |

### Step 4 — Build the Diagnosis Logic Block

Click on **Diagnosis 1** in the command canvas. This opens the visual diagnostic builder where all conditional logic is configured. The diagnosis block contains three logical sections:

#### 4.1 If (A) — No Changes Path

This is the happy-path branch. It fires when the output confirms the running and startup configs are in sync.

| Parameter | Setting | Notes |
|-----------|---------|-------|
| Variable A | `$config_differences_full` | The full text output of the parsed diff section |
| Operator | **Contains** | Checks whether the string includes the target phrase |
| Value | `!No changes were found` | IOS literal string when configs match |
| Diagnosis message | `!No changes were found` | Shown in the intent result panel; color-coded green |
| Status Code target | Intent | Sets the overall intent status, not per-device |
| Status Code icon | Green | Signals healthy — no drift detected |

#### 4.2 ElseIf (A and B) — Unsaved Changes Detected Path

This branch fires when the output does **NOT** contain `!No changes were found` **AND** the `$diff_content` variable is not empty. This double condition prevents false positives in edge cases where the command output is unexpected.

| Condition | Variable | Operator | Value |
|-----------|----------|----------|-------|
| A | `$config_differences_full` | Does not contain | `!No changes were found` |
| B | `$diff_content` | Is not empty | *(no value needed)* |

#### 4.3 Variable Transformations Inside the ElseIf Block

Before surfacing the result to the engineer, the diff content is cleaned up using two sequential **Set Variable** steps:

**Set Variable 1 — Normalize Line Breaks**

```
Variable:    change_var
Expression:  ReplacebyRegex($diff_content, "\n+", "\n", 0)
```

Purpose: The raw `$diff_content` string may contain multiple consecutive newline characters (`\n\n`) which produce blank lines in the output. This regex collapses all multi-newline sequences into a single newline, producing clean, compact output.

**Set Variable 2 — Trim Null Characters**

```
Variable:    change_var
Expression:  Trim($change_var, "")
```

Purpose: Removes any null characters or whitespace before/after the content. When the `trimchars` argument is empty (`""`), NetBrain's Trim function strips null characters and blank rows from the beginning and end of the string.

#### 4.4 Final Diagnosis Message and Status Code

After transformation, the cleaned variable is pushed to the diagnosis output:

| Setting | Value |
|---------|-------|
| Diagnosis message body | `$change_var` — displays the full list of unsaved config lines |
| Status Code target | **Device** — flags this specific device as needing attention |
| Status Code icon | **Red** — appears on the device node in the NetBrain map |
| Status Code message | `$change_var` — the diff content is also the status message |

---

## 4. Usage Flow — From Run to Remediation

The map view below shows Config Sentinel after execution. Green devices have no drift; red/amber devices have unsaved changes. The left panel displays the intent results with per-device diagnosis messages.

![NetBrain map view showing Config Sentinel results with green and red device status indicators]({{ site.baseurl }}/assets/images/sentinel/map_view.png)

Once the intent is built and published, the operational workflow for an engineer looks like this:

| Step | Action | Details |
|------|--------|---------|
| 1 | **Select Scope** | Open a NetBrain map containing the target Cisco IOS/IOS-XE devices. This can be a single site, a region, or the entire network. |
| 2 | **Run the Intent** | Click Run on Config Sentinel. NetBrain polls all devices in scope simultaneously, executing `show archive config differences` on each one. |
| 3 | **Read the Map** | Device nodes change colour. **Green** = no drift. **Red/amber** = unsaved changes requiring attention. |
| 4 | **Inspect the Device** | Click a flagged device. The intent result panel shows the exact lines that are in the running config but absent from startup. |
| 5 | **Remediate** | If changes are intentional and correct, SSH to the device and issue: `copy running-config startup-config` (or `write memory`). If unexpected, escalate for investigation. |
| 6 | **Re-run to Confirm** | After remediation, re-run the intent. The device should now return green with `!No changes were found`. |

---

The dynamic table provides a tabular summary of all devices in scope, with pass/fail status for each device at a glance.

![NetBrain dynamic table showing per-device Config Sentinel results]({{ site.baseurl }}/assets/images/sentinel/dynamic_table.png)

Results can also be exported to Excel for reporting, audit evidence, or offline review. Devices with unsaved changes are highlighted in red with the full diff content in the status column.

![Excel export of Config Sentinel results with green/red device status and diff details]({{ site.baseurl }}/assets/images/sentinel/excel_export.png)

---

## 5. Business Use Cases

Config Sentinel addresses a range of real-world operational scenarios across network teams of all sizes:

### Post-Change Control Verification

After a maintenance window, validate that all approved changes have been saved to startup config. Run the intent immediately after the change window closes. Any device flagged as red indicates a write was missed — catch it before the on-call engineer signs off.

### Post-Outage / Post-Reboot Audit

After a power event, UPS failure, or emergency reboot, quickly assess what config was lost. The intent run gives you a before/after picture of what was in running but not startup — turning a forensic investigation into a 2-minute map review.

### Network Compliance and Audit Readiness

Many compliance frameworks (SOX, PCI-DSS, ISO 27001) require that network configuration changes are documented and persistent. Running this intent before audits confirms that no shadow changes exist across the estate. Exportable results provide audit evidence.

### Estate Drift Discovery (Day 1 Assessment)

When onboarding a new customer or inheriting a network, engineers often have no idea what state the configs are in. Run Config Sentinel across all devices immediately. The result map shows the true health of the config estate in minutes — no manual spot-checks required.

### Unauthorized Change Detection

If an unauthorized or undocumented change was applied to a device's running config, Config Sentinel will surface it. The diff output shows exactly what was added or removed. Combined with NetBrain's timestamp and audit trail, this provides strong evidence for incident review.

### Scheduled Hygiene Check

Schedule the intent to run weekly (or daily on critical infrastructure) as part of a network hygiene baseline. Any device that consistently shows drift becomes a candidate for process improvement — identifying teams or individuals who routinely forget to save changes.

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
