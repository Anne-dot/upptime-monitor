# CLAUDE.md

This file provides guidance to Claude Code when working with this repository.

## MANDATORY: Read Global Instructions First

**Before starting ANY work:**

1. Read `/home/d0021/Automation/05-ai-instructions/instructions.md`
2. Read `/home/d0021/Automation/05-ai-instructions/eesti_keele_juhend.txt`
3. Read this file and `README.md`

Read files FULLY. Follow instructions step-by-step. This system helps user with ADHD - skipping steps undermines the systems that help them.

## Session Start Checklist

<session_start>
1. Check open issues and summarize for user:
   ```bash
   gh issue list --repo Anne-dot/upptime-monitor --state open
   ```

2. Ask user what they want to work on today
</session_start>

## Project Overview

GitHub Actions-based website monitoring. No server, no deploy - push to main = live.

**What it does:**
- Checks websites every 30 min (v2 workflow)
- Sends email alerts on problems (timeout, HTTP errors, PHP errors, SSL expiry)
- Sends weekly summary on Mondays

## Architecture

| File | Purpose |
|------|---------|
| `sites.json` | **Single source of truth** - all monitored sites |
| `uptime-check-v2.yml` | Active monitoring (every 30 min) |
| `weekly-summary.yml` | Monday morning report |
| `uptime-check.yml` | DEPRECATED backup (manual only) |

## Adding/Removing Sites

Edit `sites.json` only. Workflow reads it automatically.

## Debugging

<debugging_steps>
1. List recent workflow runs:
   ```bash
   gh run list --repo Anne-dot/upptime-monitor --limit 10
   ```

2. View specific run logs:
   ```bash
   gh run view <run-id> --repo Anne-dot/upptime-monitor --log
   ```

3. Check failed runs only:
   ```bash
   gh run list --repo Anne-dot/upptime-monitor --status failure --limit 5
   ```
</debugging_steps>

## Work Style

<conservative_action>
This is a monitoring system for production websites. Before making changes:

1. Show the proposed change to user
2. Wait for approval
3. Make change
4. Verify workflow still works

Do not optimize or skip steps. Each step exists for a reason.
</conservative_action>
