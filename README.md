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

