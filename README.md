# Upptime Monitor

Simple website monitoring using GitHub Actions.

## Purpose

Automated uptime monitoring that:
- Checks websites regularly (every 5-30 minutes)
- Detects issues (downtime, slow response, PHP errors, SSL expiry)
- Sends email alerts when problems occur
- Generates weekly summary reports

Free, serverless, runs entirely on GitHub Actions.

## What It Monitors

Each website is checked for:

1. **Availability** - responds within 30 seconds
2. **HTTP Status** - returns 200 OK
3. **PHP Errors** - no Fatal/Parse errors or Warnings
4. **Content** - response is not empty (<500 bytes)
5. **SSL Certificate** - more than 7 days until expiry

## Workflows

| Workflow | Description | Frequency |
|----------|-------------|-----------|
| `uptime-check.yml` | Main monitoring (11 parallel jobs) | Every 5 min |
| `uptime-check-v2.yml` | Cost-efficient version (1 job) | Every 30 min |
| `weekly-summary.yml` | Weekly report with stats | Monday 8:00 |

## Setup

### 1. Secrets (Settings → Secrets → Actions)
```
MAIL_USERNAME = your-gmail@address
MAIL_PASSWORD = gmail-app-password
```

### 2. Environment Variables (in workflow file)
```yaml
env:
  TEST_MODE: "false"  # true = only TECH_EMAIL receives alerts
  TECH_EMAIL: "your@email.com"
```

### 3. Adding Sites

Edit the `uptime-check.yml` matrix section:
```yaml
matrix:
  site:
    - url: https://your-site.com
      name: your-site.com
      emails: "contact@email.com"
      is_php: true
```

## Public vs Private Repository

| Repo Type | Actions Limit | Recommendation |
|-----------|---------------|----------------|
| PUBLIC | Unlimited | Use v1 (every 5 min) |
| PRIVATE | 2000 min/month | Use v2 (every 30 min) |

## Minutes Usage (Private Repo)

GitHub Free tier provides 2000 minutes/month for private repositories.

| Workflow | Min/run | Runs/day | Lasts |
|----------|---------|----------|-------|
| v1 (5 min) | 11 | 288 | ~5 days ❌ |
| v2 (30 min) | 1 | 48 | ~41 days ✅ |

## Features

- **Matrix Strategy** - parallel checks for faster results
- **Per-site Email Alerts** - each site owner gets only their alerts
- **SSL Monitoring** - warns before certificates expire
- **Response Time Tracking** - weekly report shows site speed
- **Incident Tracking** - weekly report shows failure count
- **Cost Optimization** - v2 uses 11x fewer GitHub Actions minutes

## Tech Stack

- GitHub Actions (CI/CD)
- Bash scripting
- cURL for HTTP checks
- OpenSSL for certificate checks
- Gmail SMTP for notifications
