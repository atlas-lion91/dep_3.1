# Post-Incident Report

## Problem:
- Our application experienced an 13-minute downtime in a single day, which exceeds our SLA allowance of only 15 minutes per year.

## Objective:
-  Our goal is to prevent future downtime and significantly reduce downtime in areas where it cannot be completely prevented.

## Incident Summary:
- The URL shortener application went down due to a recent update made by a new team member. This update caused a 500 internal server error, rendering the website inaccessible.

## Root Cause:
- The incident occurred because the new team member committed version 2 of our application to the main branch, which contained an incorrect usage of a JSON method. They used "json.loads(urls_file)" instead of the correct "json.load(urls_file)."

## Duration of Downtime:
- The website was down for a total of 13 minutes.

## Resolution Steps:

### Step 1: Log Analysis
- **Analysis**: We examined Elastic Beanstalk logs to identify errors using the following Bash command:
  ```bash
  eb logs | grep -C 3 'error' | nl -w3 -s':' | sort -u -k2,2 | sort -n -k1,1 > error_hunt_filtered.txt
