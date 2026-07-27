---
name: New experiment / PoC
about: Propose a new lab experiment or PoC for an existing module
title: "[Experiment] <module> — <technique>"
labels: experiment
assignees: ""
---

## Objective

<!-- What does this experiment demonstrate? One or two sentences. -->

## Module

<!-- e.g. remote-thread / apc / thread-hijack / image-tampering — and the planned experiment ID (R4, A4, ...) -->

## Scope checklist

- [ ] Runs only inside the isolated lab (see `lab/setup.md`)
- [ ] Uses the project-standard harmless payload (MessageBox) — no real payloads
- [ ] Target is the lab dummy process or a lab-owned binary
- [ ] Includes a documentation note in `docs/` (technique, prerequisites, expected behavior)
- [ ] No obfuscation, anti-analysis, or EDR evasion (out of scope for this repo)
- [ ] Mapped to MITRE ATT&CK (technique + sub-technique)

## Lab requirements

<!-- Hardening/observation stage, snapshots, tooling, privileges needed -->

## Acceptance criteria

- [ ] Experiment executes and produces the documented result in the lab
- [ ] Telemetry generated is recorded (Sysmon event IDs, alerts)
- [ ] Results documented in `docs/` (observed vs. expected)
- [ ] Compiled binaries are gitignored and discarded after the run
