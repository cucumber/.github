Incident Response Plan
======================

This document describes how we plan to act should there be a security incident.
Most of these steps are hopefully obvious, but writing these down will
hopefully provide some structure when things do get stressful.

# Principles

* Protection: Act to minimize harm and provide guidance.
* Transparency: Document incidents and fixes.
* Stewardship: Ensure the ongoing protection of users and the project.
* Pragmatism: Do what is possible in the available time with the available people.

# How we handle incidents

## Preparation

* Stand up. 
* Walk away from the PC.
* Get something to drink.
* Breathe.

## Detection

* We monitor security reports sent through [coordinated disclosure](SECURITY.md).
* If we spot a bug or report that looks like a security risk, we treat it as an incident.

## Assessment

* Check the severity:
    * Critical: package or repo compromised, malicious code, supply chain attack.
    * High: Vulnerabilities that allow code execution, XSS, or leak secrets.
    * Medium: Denial of service, memory leaks, or integrity issues.
    * Low: Docs defacement, minor regressions.

## Response

* Acknowledge the report (privately if sensitive, publicly if not).
* For critical/high issues:
    * Rotate any exposed secrets/tokens.
    * Patch the bug or vulnerability.
    * Deprecate or yank affected packages and publish a patch+1 version, if needed.
    * Rebuild and redeploy docs/site from a clean commit.

* For medium/low issues:
    * Patch and document the fix.

## Communication

* Disclose vulnerabilities to our enterprise partners through Tidelift
* Create an issue or pull request with
    * the summary of the incident
    * the fix (if any)
    * upgrade and mitigation steps for users (if any)
* Include an entry under the `### Security` header in the `CHANGELOG`.
* For critical issues write a blog post

## Recovery & Remediation

* After fixing, review what happened and update this process if needed.
* Add tests or automation to prevent similar issues.
* Rotate credentials and check repo and package security settings.
