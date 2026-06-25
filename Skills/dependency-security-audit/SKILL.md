---
name: dependency-security-audit
description: Audits npm and Python project dependencies for known vulnerabilities, supply chain risks, and maintenance issues. Use this skill when a security scan or dependency review is requested.
---

### Purpose

This skill performs an automated security audit of project dependencies. It identifies vulnerable, outdated, or unmaintained npm and Python packages, prioritizes risks, and produces actionable remediation guidance suitable for testers, developers, and CI pipelines.


### What This Skill Does

1. Detects supported ecosystems (Node.js and/or Python)
2. Extracts direct and transitive dependencies
3. Runs trusted vulnerability scanners
4. Correlates and deduplicates findings
5. Prioritizes issues by severity and exploitability
6. Explains risks in clear, tester-friendly language with OWASP and CWE mappings
7. Suggests safe upgrade or mitigation paths
8. Determines pass or fail status for CI usage based on configurable severity thresholds

---

### Step-by-Step Instructions

#### 1. Detect Project Type

* Check for `package.json` to identify Node.js projects
* Check for `requirements.txt`, `poetry.lock`, `Pipfile.lock`, `uv.lock`, `pdm.lock`, or `pyproject.toml` to identify Python projects
  * Modern package managers: `uv`, `poetry`, `pdm`, `hatch`
  * Legacy: `pip`, `pipenv`

#### 2. Extract Dependencies

* For Node.js, read dependency and lock files to determine exact versions
* For Python, resolve installed or locked package versions:
  * **uv projects**: Generate requirements from lock file using `uv pip compile pyproject.toml -o requirements.txt`
  * **poetry projects**: Use `poetry export -f requirements.txt -o requirements.txt`
  * **pdm projects**: Use `pdm export -o requirements.txt`
  * **pip/pipenv**: Use existing `requirements.txt` or `Pipfile.lock`

#### 3. Run Vulnerability Scans

**Scanner Installation (if not present):**
* Python: `python3 -m pip install pip-audit`
* OSV Scanner: `brew install osv-scanner` (macOS) or download from GitHub releases
* If installation fails due to missing network access (e.g. air-gapped or restricted CI environments), stop and notify the user that scanners could not be installed and manual intervention is required before the audit can proceed.

Run ecosystem-appropriate scanners:

* Node.js:
  * `npm audit`
  * `osv-scanner` using lock files
* Python:
  * `pip-audit -r requirements.txt -f json --desc on` (for requirements.txt)
  * `pip-audit --locked .` (for poetry/pipenv projects)
  * For **uv.lock**: First export to requirements.txt, then run pip-audit
  * `osv-scanner scan --lockfile=requirements.txt --format json`

**Important:** pip-audit does not natively support `uv.lock` — must convert to requirements.txt first.

Treat scanner output as authoritative vulnerability data.

#### 4. Analyze and Correlate Results

* Deduplicate overlapping CVEs
* Prioritize HIGH and CRITICAL issues
* Distinguish between production and development dependencies
* Identify transitive dependency risks
* Flag unmaintained packages even if no CVE exists

#### 5. Explain Risks and Map to OWASP & CWE

For each confirmed issue:

* Describe what the vulnerability is
* Explain why it matters in practical terms
* Identify whether it is directly or transitively used
* Map the issue to the relevant **OWASP Top 10 (2025)** category using `references/owasp-mapping.md`
* Map the issue to the most specific applicable **CWE identifier** using `references/owasp-mapping.md`
* Prefer specific, mappable CWEs over high-level category CWEs — see the mapping reference for guidance on which CWEs are PROHIBITED for use

#### 6. Recommend Actions

* Suggest minimal safe upgrades
* Identify breaking change risks
* Recommend alternative packages if maintenance is abandoned

#### 7. Determine CI Pass/Fail Status

* **Default threshold: FAIL if any HIGH or CRITICAL severity finding is present in production dependencies**
* MEDIUM and LOW findings result in a PASS with warnings unless overridden
* Development-only dependency findings do not trigger a FAIL by default
* The threshold can be overridden by the user (e.g. fail on MEDIUM and above)
* Always state the threshold used at the top of the report

#### 8. Format the Output

Structure your output following the format defined in the **Example Output** section below. Every report must include: overall status, severity counts, high-risk findings with OWASP and CWE mappings, medium/low findings, observations, and a final recommendation.

---

### Example Output

Dependency Security Audit Summary

Overall status: **FAIL**  
CI threshold: High and Critical (default)

Critical issues: 1  
High issues: 2  
Medium issues: 3  
Low issues: 5  

---

#### High-Risk Findings

1. **lodash 4.17.20** (transitive dependency)  
   - Issue: Prototype pollution vulnerability  
   - Why this matters: Malicious input could modify application objects and alter runtime behavior  
   - OWASP mapping: A03:2025 Software Supply Chain Failures  
   - CWE: CWE-1321 Improperly Controlled Modification of Object Prototype Attributes  
   - Recommended action: Upgrade to lodash 4.17.21  

2. **urllib3 1.25.x** (direct dependency)  
   - Issue: Improper TLS certificate validation  
   - Why this matters: Encrypted connections may be vulnerable to man-in-the-middle attacks  
   - OWASP mapping: A04:2025 Cryptographic Failures  
   - CWE: CWE-295 Improper Certificate Validation  
   - Recommended action: Upgrade to urllib3 2.x  

---

#### Medium-Risk Findings

- **minimist**  
  - Issue: Input handling weakness in CLI argument parsing  
  - Recommended action: Safe if no user-controlled input; review usage

---

#### Observations

- Several vulnerabilities originate from **transitive dependencies**  
- All high-risk issues have **safe upgrade paths available**  
- No malicious packages detected  

---

#### Final Recommendation

Do not release to production until all **High and Critical** issues are resolved.  
After upgrading, rerun the dependency security audit to confirm remediation.


---

### Common Edge Cases

* Vulnerabilities present only in development dependencies
* CVEs with no available fix
* False positives in unused transitive dependencies
* Packages with security issues but no recent maintenance activity
* Projects containing both Node.js and Python dependencies
* **Modern Python package managers** (uv, pdm, hatch) with non-standard lock files:
  * Convert to requirements.txt format before running pip-audit
  * Use `uv pip compile` for uv.lock, `pdm export` for pdm.lock, etc.
* **Missing scanners**: Install pip-audit and osv-scanner if not available; if network access is restricted, notify the user and halt
* **Zero vulnerabilities**: Still provide a comprehensive report on package versions and maintenance status

Handle these cases by prioritizing exploitability and production impact.

---

### IMPORTANT

* Do not invent vulnerabilities
* Do not assume exploitability without scanner evidence
* Prefer clarity over security jargon
* This skill functions as a **security quality gate** — its purpose is to enforce a reviewable, evidence-based pass/fail decision on dependency risk, not to replace a full penetration test

---

### References

* Use `references/owasp-mapping.md` to map findings to OWASP Top 10 (2025) categories and CWE identifiers
* Consult this reference during Step 5 when classifying each finding