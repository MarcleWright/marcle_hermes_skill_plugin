---
name: marcle-family-hermes-backup-rhythm
description: Use when managing Marcle's family Hermes backup system: scheduled quick/full/archive backups, retention, naming rules, storage layout, verification, and future adjustments.
version: 1.0.0
author: Hermes Agent
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [backup, hermes, profiles, family, recovery, windows]
    related_skills: [hermes-agent]
---

# Marcle Family Hermes Backup Rhythm

## Overview

This skill documents Marcle's production backup setup for a multi-profile family Hermes deployment on Windows. It covers the backup root folder, naming conventions, retention policy, scheduled-task names, implementation script, and the practical rules for adjusting or verifying the system later.

This is the source of truth for the current family backup design.

## When to Use

Use this skill when:
- You need to inspect, explain, or adjust the family backup setup
- You need to verify whether scheduled backups are still configured correctly
- You want to change backup times, retention windows, naming formats, or folder layout
- You need to manually run one backup tier
- You need to understand where logs and generated zip files are stored
- You need to prepare for recovery or backup audits

Do not use this skill for:
- Restoring a specific profile from backup without first inspecting the target zip
- Editing Hermes core backup implementation inside the hermes-agent repo
- One-off ad hoc file copies unrelated to the family Hermes system

## Current Production Setup

### Root folder

```text
D:\Documents_Marcle\hermes_profiles_backup
```

### Folder structure

```text
D:\Documents_Marcle\hermes_profiles_backup\
├─ daily-quick\YYYY\MM\
├─ weekly-full\YYYY\MM\
├─ monthly-archive\YYYY\MM\
├─ restore-test\logs\
├─ restore-test\temp\
├─ scripts\
└─ README-backup-policy.txt
```

### Naming rules

- Daily quick:
  ```text
  hermes-quick-family-YYYYMMDD-HHMM.zip
  ```
- Weekly full:
  ```text
  hermes-full-family-YYYYMMDD-HHMM.zip
  ```
- Monthly archive:
  ```text
  hermes-archive-family-YYYYMM01-HHMM.zip
  ```

## Backup Tiers

### 1. Daily quick
- Time: every day at **01:00**
- Purpose: recent rollback protection
- Retention: **14 days**
- Output family: `daily-quick`

### 2. Weekly full
- Time: every **Sunday at 03:00**
- Purpose: main family recovery point
- Retention: **56 days / 8 weeks**
- Output family: `weekly-full`

### 3. Monthly archive
- Time: day **1** of every month at **04:00**
- Purpose: long-term historical archive
- Retention: **366 days / about 12 months**
- Output family: `monthly-archive`

## Scheduled Task Names

```text
HermesBackupDailyQuick
HermesBackupWeeklyFull
HermesBackupMonthlyArchive
```

## Implementation Files

### Policy document

```text
D:\Documents_Marcle\hermes_profiles_backup\README-backup-policy.txt
```

### Execution script

```text
D:\Documents_Marcle\hermes_profiles_backup\scripts\hermes_backup_job.py
```

### Python interpreter used by tasks

```text
C:\Users\MarcleW\AppData\Local\hermes\hermes-agent\venv\Scripts\python.exe
```

## How the Script Works

### Quick mode
Quick mode calls:

```bash
hermes backup --quick --label daily
```

Hermes quick backup natively creates a snapshot directory rather than the final family zip structure, so the wrapper script must:
1. run the quick snapshot command
2. parse the created snapshot id from stdout
3. locate the snapshot under:
   ```text
   C:\Users\MarcleW\AppData\Local\hermes\state-snapshots\
   ```
4. package that snapshot directory into the family zip naming/layout under `daily-quick`
5. validate the generated zip
6. prune expired quick zips and old `*-daily` snapshot folders

### Full / archive mode
These call Hermes backup directly with an output path and then:
1. verify the zip exists
2. validate the zip contents
3. prune expired zips for that class

## Manual Run Commands

### Quick
```bash
"/c/Users/MarcleW/AppData/Local/hermes/hermes-agent/venv/Scripts/python.exe" /d/Documents_Marcle/hermes_profiles_backup/scripts/hermes_backup_job.py quick
```

### Full
```bash
"/c/Users/MarcleW/AppData/Local/hermes/hermes-agent/venv/Scripts/python.exe" /d/Documents_Marcle/hermes_profiles_backup/scripts/hermes_backup_job.py full
```

### Archive
```bash
"/c/Users/MarcleW/AppData/Local/hermes/hermes-agent/venv/Scripts/python.exe" /d/Documents_Marcle/hermes_profiles_backup/scripts/hermes_backup_job.py archive
```

## Verification Checklist

After any change, verify all of the following:

- [ ] `README-backup-policy.txt` still matches actual scheduled-task behavior
- [ ] `hermes_backup_job.py` exists and is readable
- [ ] task names still exist in Windows Task Scheduler
- [ ] task schedules match 01:00 / Sunday 03:00 / day-1 04:00
- [ ] a manual `quick` run produces a zip under `daily-quick\\YYYY\\MM\\`
- [ ] logs are written to `restore-test\\logs\\`
- [ ] retention pruning still works as intended
- [ ] zip file validates successfully

## Common Adjustment Workflow

When changing the backup system:

1. Read:
   - `README-backup-policy.txt`
   - `scripts/hermes_backup_job.py`
2. Decide whether the change is:
   - schedule only
   - retention only
   - naming/layout only
   - Hermes command behavior
3. Update the script first if logic changes are needed
4. Update the policy document so it stays authoritative
5. Recreate or update scheduled tasks if schedule or command changed
6. Run one manual test for the affected tier
7. Confirm output path, log file, and retention behavior

## Common Pitfalls

1. **Assuming `hermes backup --quick -o ...` behaves like full backup**
   It does not. Quick backup is snapshot-oriented, so the wrapper script is required for the family zip layout.

2. **Changing the schedule but not the documentation**
   Always update `README-backup-policy.txt` together with Task Scheduler changes.

3. **Editing only Task Scheduler and forgetting the script contract**
   If arguments or output rules changed, task edits alone are insufficient.

4. **Treating the backup folder as non-sensitive**
   These zips may include `.env`, `auth.json`, tokens, config, and family data. Handle as sensitive backup material.

5. **Skipping a manual verification after changes**
   Always manually run at least the affected tier once.

## Recovery Notes

- Whole-family backup is the primary strategy.
- Per-profile export is optional, not required for the current plan.
- Monthly archives should ideally also be copied to another storage target later (NAS/cloud/external disk) for extra safety.

## Quick Reference

### Backup root
```text
D:\Documents_Marcle\hermes_profiles_backup
```

### Primary policy file
```text
D:\Documents_Marcle\hermes_profiles_backup\README-backup-policy.txt
```

### Script
```text
D:\Documents_Marcle\hermes_profiles_backup\scripts\hermes_backup_job.py
```

### Scheduled tasks
```text
HermesBackupDailyQuick
HermesBackupWeeklyFull
HermesBackupMonthlyArchive
```
