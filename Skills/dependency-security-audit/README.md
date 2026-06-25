# dependency-security-audit

Audits npm and Python project dependencies for known vulnerabilities, supply chain risks, and maintenance issues. Trigger it by requesting a security scan or dependency review.

## What it does:
* Detects Node.js (package.json) and/or Python ecosystems
* Extracts direct and transitive dependencies
* Runs npm audit, pip-audit, and osv-scanner
* Deduplicates and prioritizes findings by severity
* Maps each issue to OWASP Top 10 (2025) and CWE identifiers
* Recommends safe upgrade or mitigation paths
* Produces a CI pass/fail verdict (default: FAIL on any HIGH or CRITICAL in production deps)

## Reference files:
* owasp-mapping.md
* owasp-mapping-accuracy-report.md
