# Salesforce Customer Success Onboarding System

## Overview
This project demonstrates a Salesforce-based Customer Success onboarding and lifecycle management system designed to support education and SaaS customers after a deal closes.

The system automates onboarding kickoff, task creation, ownership assignment, and customer health tracking using Salesforce Flow.

## Problem
Customer Success teams often struggle with:
- Inconsistent onboarding
- Duplicate follow-up tasks
- Limited visibility into customer health
- Reactive retention efforts

## Solution
I designed and implemented a record-triggered Salesforce Flow that:
- Triggers when an Opportunity is marked Closed Won
- Starts onboarding once using guardrail logic
- Assigns Customer Success ownership
- Creates structured onboarding tasks
- Tracks onboarding stage and health status at the Account level

## Key Features
- Record-triggered Flow with idempotent logic
- Account-level onboarding and health fields
- Automated task creation for CS milestones
- Dashboards for health and workload tracking
- Designed to prevent duplicate onboarding actions

## Edge Cases Handled
- Opportunity edits after Closed Won
- Multiple saves and re-runs
- Multiple Opportunities per Account
- Safe reactivation without duplication

## Tools Used
- Salesforce (Developer Org)
- Salesforce Flow
- Reports & Dashboards

## Why This Matters
This system enables proactive onboarding, clearer ownership, and scalable Customer Success operations — especially relevant for education and SaaS environments.

## Data model (Account fields)
This project adds a lightweight CS layer to the **Account** object:

- `CSM Owner` (Lookup → User)
- `Onboarding Stage` (Picklist)
- `Health Status` (Picklist: Green/Yellow/Red)
- `Adoption Score` (Picklist: Low/Medium/High)
- `Last Check-in Date` (Date)
- `Go-Live Date` (Date)
- `Onboarding Started` (Checkbox, default = false)

---

## Flow logic (step-by-step)
**Trigger:** Opportunity is updated and Stage changes to **Closed Won**

### 1) Fetch Account
- Get Account by `{!$Record.AccountId}`

### 2) Guardrail A — start onboarding only once
- If `Account.Onboarding_Started__c = TRUE` → **stop**
- If `FALSE` → proceed

### 3) Initialize CS fields on Account
- set `Onboarding_Started__c = TRUE`
- set `Onboarding Stage = 1 - Kickoff Scheduled`
- set `Health Status = Yellow`
- set `Last Check-in Date = Today`
- set `CSM Owner = current user (demo)`

### 4) Guardrail B — prevent duplicate kickoff task
- Get existing Task where:
  - `WhatId = AccountId`
  - `Subject = "CS Kickoff Call"`
- If it exists → skip
- If not → create it

### 5) Create onboarding tasks (milestones)
Example tasks:
- `CS Kickoff Call` (Due +2 days)
- `Provision Access / Verify Rosters` (Due +5 days)
- `Admin + Educator Training Session` (Due +10 days)
- `30-Day Adoption Check-in` (Due +30 days)
---

## Edge cases handled
- Opportunity edits after Closed Won (no duplicate tasks)
- repeated saves (safe)
- multiple Closed Won opportunities per Account (safe)
- accidental re-triggering (task existence checks prevent duplication)

