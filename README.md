# Post-Incident Report

## Problem:
- Our application experienced an 13-minute downtime in a single day, which exceeds our SLA allowance of only 15 minutes per year.

## Objective:
-  Our objective is to minimize future downtime and greatly diminish any downtime that may occur in specific situations.

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
  eb logs | awk '/error/ {print NR":"$0}' | sort -u -k2,2 | sort -n -t':' -k1,1 > error_hunt_filtered.txt
- We filtered logs for lines containing "error" and their context.
- We removed duplicate lines.
- Logs were sorted based on their original order.

### Step 2: Code Inspection

- We searched the logs for occurrences of the JSON method mentioned in the logs and found issues related to "json.loads()" in our "application.py" file. To identify the change that caused the issue, we compared the current version of "application.py" with the last working version using the command:
  ```bash
  git log -p
- This revealed that version 1 used "json.load()" while version 2 used "json.loads()"

### Step 3: Code Correction
- In "application.py," we corrected the JSON method usage, ensuring it matched the correct format.

### Step 4: Testing 
- We verified the functionality of our URL shortening feature to ensure it was working correctly.

## Incident Resolution:
The incident was fully resolved after identifying and correcting the error in the "application.py" file.

# Preventive Measures to Avoid Similar Incidents

To prevent a similar incident from happening again, consider implementing the following preventive measures and improvements based on the lessons learned from the post-incident report:

1. **Code Review and Testing:**
   - **Code Review Process**: Strengthen your code review process to catch potential issues early. Encourage team members to review each other's code, with a focus on best practices and error-prone areas.
   - **Comprehensive Testing**: Continue to enforce comprehensive testing, including unit tests, integration tests, and user acceptance tests, to ensure code changes do not introduce critical errors.

2. **CI/CD Pipeline Enhancements:**
   - **Static Code Analysis**: Implement static code analysis tools in your CI/CD pipeline to automatically identify code quality and security issues before deployment.
   - **Automated Testing**: Integrate automated testing and quality assurance checks at multiple stages of your CI/CD pipeline, including staging environments, to catch issues early.

3. **Version Control Practices:**
   - **Branch Policies**: Enforce branch policies that require code changes to pass tests and code reviews before being merged into the main branch.
   - **Continuous Monitoring**: Continuously monitor your version control system for changes that introduce errors or breakages.

4. **Incident Response Automation:**
   - **Automated Rollbacks**: Implement automated rollback mechanisms in your production environment that can quickly revert to the last known working version in case of issues.
   - **Automated Alerts**: Configure automated alerting systems that notify your team immediately when an incident occurs, allowing for faster response.

5. **Root Cause Analysis:**
   - **Five Whys Analysis**: Continue to use the Five Whys technique to investigate the root causes of incidents. This helps identify systemic issues and areas for improvement.
   - **Documentation**: Maintain a comprehensive incident database with detailed information about incidents, their causes, and resolutions.

6. **Preventive Fixes:**
   - **Proactive Fixes**: Implement preventive code fixes in your CI/CD pipeline to address common issues before they make their way into production.
   - **Automated Testing**: Include automated tests that specifically check for known issues that have caused incidents in the past.

7. **Monitoring and Alerting:**
   - **Advanced Monitoring**: Enhance your application and server monitoring to detect issues early. Implement proactive monitoring for potential error conditions.
   - **Threshold Alerts**: Set up threshold-based alerts that trigger when certain performance metrics or error rates exceed acceptable limits.

8. **Disaster Recovery and Redundancy:**
   - **Redundancy**: Ensure critical components of your infrastructure, such as databases and servers, have redundancy and failover mechanisms in place.
   - **Regular Backups**: Maintain regular backups of your data and configurations to minimize data loss in case of an incident.

9. **Security Measures:**
   - **Security Audits**: Conduct regular security audits and vulnerability assessments to identify and address potential security risks.
   - **Security Training**: Train your team on secure coding practices and provide ongoing security awareness training.

10. **Continuous Improvement:**
    - **Retrospectives**: Hold regular post-incident retrospectives to assess how well your preventive measures are working and identify areas for further improvement.
    - **Iterative Approach**: Continuously refine and adapt your preventive measures based on lessons learned from incidents.

By implementing these preventive measures and continually refining your processes, we can significantly reduce the likelihood of similar incidents occurring in the future and enhance the resilience of our system.






