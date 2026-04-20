---
title: "Config Sentinel — unsaved configuration drift detection"
tags: [netbrain, cisco, ios, automation, configuration-management, network-intent]
excerpt_summary: "A NetBrain Network Intent that flags devices whose running config has drifted from startup — no scripts, no extra tooling, green/red status on the map."
---

A NetBrain Network Intent solution for automatically detecting configuration drift between running and startup configurations on Cisco IOS / IOS-XE devices.

## The problem this solves

- A device reloads after a power event. The running config had 3 weeks of undocumented, unsaved changes. Everything is gone.
- A change control was applied but the engineer was paged mid-task and never ran `write mem`. The audit shows clean.
- A network team inherits an environment with unknown drift across 200 devices. There is no fast way to assess.

## What Config Sentinel delivers

- **Automated per-device drift detection at scale** — across hundreds of devices in a single run.
- **Clear diff output** showing exactly what lines are in running config but absent from startup.
- **Green / red per-device health status** surfaced directly in the NetBrain map.
- **Zero manual SSH required** — fully integrated into the NetBrain intent workflow.

## How it works

Config Sentinel is built as a two-step NetBrain Network Intent: a data retrieval command block followed by a diagnosis logic block. The two components are wired together via parsed variables that carry configuration data from device output into the diagnostic engine.

### High-level flow

```
1 Device Poll → 2 Command Execute → 3 Regex Parse → 4 Variable Extract → 5 Logic Evaluate → 6 Diagnosis Message → 7 Map Status
```

### Component breakdown

The intent is composed of two numbered command entries in the NetBrain command canvas:

| Step | Component | Purpose |
|------|-----------|---------|
| 1 | **Switch — show archive config differences** | Executes the IOS command that outputs the diff between running and startup config. Native to IOS / IOS-XE, no additional configuration on the device. |
| 1.1 | **Parser: Format1 (Baseline)** | Applies a regex-based variable extraction pattern to the raw command output. Pulls out four key variables: `$whole_output_header`, `$config_differences_full`, `$diff_header`, and `$diff_content`. |
| 1.2 | **Diagnosis 1** | Wires the parsed variables into the diagnostic logic engine. All conditional logic, variable transformations, and status messages are defined here. |
| 2 | **Intent — \<Intent\>** | The second command slot is reserved for the Intent-level logic block, used to set Global Variables and carry information across devices or sub-intents if needed. |
| 2.1 | **Global Variables (1)** | Stores the final `result` variable at intent scope, making it available for reporting or cross-device intent logic. |

## Results on the map

The dynamic table provides a tabular summary of all devices in scope, with pass / fail status for each device at a glance.

<!-- Image pending: dynamic_table.png
![NetBrain dynamic table showing per-device Config Sentinel results]({{ site.baseurl }}/assets/images/sentinel/dynamic_table.png)
-->

Results can also be exported to Excel for reporting, audit evidence, or offline review. Devices with unsaved changes are highlighted in red with the full diff content in the status column.

<!-- Image pending: excel_export.png
![Excel export of Config Sentinel results with green/red device status and diff details]({{ site.baseurl }}/assets/images/sentinel/excel_export.png)
-->

## Business use cases

Config Sentinel addresses a range of real-world operational scenarios across network teams of all sizes:

- **Post-change-control verification** — confirm every touched device was saved before closing the ticket.
- **Post-outage / post-reboot audit** — immediately see which devices came back with a config older than what was running pre-event.
- **Estate drift discovery (day-one assessment)** — run once on a new environment to surface every device sitting on unsaved state.
- **Unauthorised change detection** — flag devices where running diverges from startup outside a known change window.
- **Scheduled hygiene check** — run nightly and alert only on the exceptions.

## Design decisions & lessons learned

| Decision | Rationale |
|----------|-----------|
| **Why `show archive config differences` vs a manual diff?** | This IOS-native command is the cleanest, most reliable source of truth for config drift. It leverages the IOS archive subsystem and does not require any additional tooling or feature configuration on the device. |
| **Why double-condition the ElseIf (A and B)?** | Testing only for "does not contain `!No changes were found`" could produce false positives if the command fails or returns unexpected output. The second condition (`$diff_content` is not empty) ensures we only flag devices where we have actual diff content to display. |
| **Why use ReplacebyRegex before Trim?** | Raw IOS output often contains runs of `\n\n` characters between config blocks. Without collapsing these first, Trim would not clean the output fully. The sequence matters: normalise first, then trim edges. |
| **Why set Status Code to Device (not Intent) in the ElseIf branch?** | The intent should flag individual devices, not the entire run. If 198 out of 200 devices are clean, an intent-level red would be misleading. Device-level status ensures the map accurately reflects which specific nodes need attention. |
| **Why use Intent Template mode?** | Intent Templates are shareable across customer environments and can be applied to device groups. Reusable across Cisco IOS / IOS-XE devices in any project without rebuild. |

## Extensibility & future enhancements

Config Sentinel is intentionally scoped to Cisco IOS / IOS-XE in v1.0. The following extensions are planned or recommended:

| Enhancement | Description |
|-------------|-------------|
| **Multi-vendor support** | Extend to Aruba / HP (`display saved-configuration`), Palo Alto (`show config diff`), Juniper (`show \| compare rollback`), and NX-OS using vendor-specific command blocks within the same intent template. |
| **Line-count metric** | Add a variable that counts the number of unsaved lines in `$diff_content`. Surface this as a numeric metric in the diagnosis message to help prioritise remediation by severity. |
| **Scheduled automation** | Wire the intent into a NetBrain automation schedule. Run nightly across all production devices and push results to a SIEM or ticketing system via the NetBrain Automation API. |
| **Save to incident** | Enable the "Save to Incident" checkbox in the Diagnosis Message block so NetBrain automatically attaches the diff output to incident tickets for flagged devices. |
| **Remediation runbook link** | Add a hyperlink in the diagnosis message pointing to the internal wiki page for the `write memory` procedure, giving NOC staff a direct path to resolution from the intent result panel. |

## Summary

Config Sentinel provides a lightweight, zero-dependency solution to one of the most common and costly operational oversights in network management: unsaved configuration changes.

Built entirely within the NetBrain intent framework — no external scripts, no third-party tooling, no additional infrastructure — it gives any engineering team instant visibility into config drift across their Cisco estate.

Build time for a fully functional intent is under 30 minutes. The operational value is immediate.

---

| | |
|---|---|
| **Document type** | Solution design & build guide |
| **Author** | Alejandro Marin — Professional Services Engineer |
| **Platform** | NetBrain Technologies (Network Intent Framework) |
| **Target devices** | Cisco IOS / IOS-XE (switch, router) |
| **Version** | 1.0 |
| **Date** | March 2026 |
