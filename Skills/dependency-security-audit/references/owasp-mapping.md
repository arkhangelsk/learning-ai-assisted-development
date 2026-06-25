# Dependency Security Findings: OWASP Top 10 (2025) and CWE Mapping

This reference maps common dependency-related security findings to the **OWASP Top 10: 2025** categories and **MITRE CWE identifiers**, supporting consistent classification and reporting in dependency security audits.

> Sources: [OWASP Top 10:2025](https://owasp.org/Top10/2025/) | [MITRE CWE Database](https://cwe.mitre.org/)

---

## OWASP Top 10: 2025 Categories

1. **A01:2025 Broken Access Control** – unauthorized actions; also encompasses SSRF (absorbed from A10:2021).
2. **A02:2025 Security Misconfiguration** – insecure defaults, exposed components. Key CWEs: CWE-16, CWE-611.
3. **A03:2025 Software Supply Chain Failures** – dependency, build system, distribution, and CI/CD weaknesses. Expands 2021 "Vulnerable and Outdated Components" to the full supply chain. Official CWEs: CWE-447, CWE-477, CWE-1035, CWE-1104, CWE-1329, CWE-1357, CWE-1395.
4. **A04:2025 Cryptographic Failures** – insecure or outdated cryptography. Key CWEs: CWE-327, CWE-331, CWE-338.
5. **A05:2025 Injection** – untrusted input influencing commands/queries. Key CWEs: CWE-77, CWE-89.
6. **A06:2025 Insecure Design** – architectural weaknesses.
7. **A07:2025 Authentication Failures** – weak authentication/session control.
8. **A08:2025 Software or Data Integrity Failures** – tampering of code/data. Key CWEs: CWE-494, CWE-502, CWE-829.
9. **A09:2025 Security Logging and Alerting Failures** – insufficient monitoring. Key CWE: CWE-778.
10. **A10:2025 Mishandling of Exceptional Conditions** – insecure error/exception management. New in 2025.

---

## Mapping Dependency Findings to OWASP & CWE

### A03:2025 Software Supply Chain Failures

Dependencies with known vulnerabilities, compromised packages, or CI/CD misconfigurations fall here.

| Finding Type | CWE | Notes |
|---|---|---|
| Known vulnerable package | CWE-1395 — Dependency on Vulnerable Third-Party Component | External library introduces known CVE risk. Primary CWE for this scenario. |
| Malicious or typosquatted package | CWE-1357 — Reliance on Insufficiently Trustworthy Component | Package source cannot be adequately trusted; may execute malicious code. |
| Vulnerable transitive dependency | CWE-1104 — Use of Unmaintained Third-Party Components or CWE-1395 | Prefer CWE-1395 if a CVE is present; CWE-1104 if the component is unmaintained/abandoned. |
| Obsolete or deprecated dependency | CWE-477 — Use of Obsolete Function or CWE-1329 — Reliance on Component That is Not Updateable | Use CWE-1329 if the component cannot be patched or updated. |

> **Note:** CWE-1035 is in the official A03:2025 list but is a Category-level CWE; MITRE discourages using it for vulnerability mapping — prefer CWE-1395 instead.

---

### A02:2025 Security Misconfiguration

Misconfigured dependency ecosystems, such as insecure build/tool settings or weak defaults.

| Finding Type | CWE | Notes |
|---|---|---|
| Dependency configuration issues | CWE-16 — Configuration or CWE-15 — External Control of System or Configuration Setting | CWE-16 is broad but active and officially mapped to A02. Prefer specific descendants when available. |
| Insecure CI/CD environment settings | CWE-489 — Active Debug Code or CWE-15 — External Control of System or Configuration Setting | Choose the CWE that best fits the specific misconfiguration. |

---

### A04:2025 Cryptographic Failures

Dependency use of insecure or deprecated cryptography.

| Finding Type | CWE | Notes |
|---|---|---|
| Insecure crypto library | CWE-327 — Use of a Broken or Risky Cryptographic Algorithm | Library uses weak or obsolete ciphers (e.g., MD5, SHA-1, DES). |
| Poor crypto defaults | CWE-327 or a specific CWE-327 descendant | Note: CWE-310 is **prohibited** per MITRE — use CWE-327 or its descendants. |
| Insufficient entropy / weak PRNG | CWE-331 — Insufficient Entropy or CWE-338 — Use of Cryptographically Weak PRNG | Weak random number generation in library defaults. |
| Improper certificate validation | CWE-296 — Improper Following of a Certificate's Chain of Trust or CWE-297 — Improper Validation of Certificate with Host Mismatch | Use CWE-296 for chain-of-trust failures; CWE-297 for hostname mismatch. |

---

### A05:2025 Injection

| Finding Type | CWE | Notes |
|---|---|---|
| CLI argument injection via package | CWE-77 — Command Injection | Inputs can alter build or shell commands through dependency APIs. |
| ORM/DB injection through dependency | CWE-89 — SQL Injection | Untrusted input passed through a dependency API reaches a database query. |

---

### A08:2025 Software or Data Integrity Failures

When dependencies or delivery mechanisms allow tampering or introduce unverified code.

| Finding Type | CWE | Notes |
|---|---|---|
| Unverified imports / no integrity check | CWE-494 — Download of Code Without Integrity Check | No signature, hash, or lock verification on downloaded dependencies. |
| Tampered or maliciously modified packages | CWE-829 — Inclusion of Functionality from Untrusted Control Sphere | Attackers modify released artifacts; supply chain tampers with package contents. |
| Insecure deserialization in dependency | CWE-502 — Deserialization of Untrusted Data | Dependency deserializes untrusted input without validation. |

---

### A01, A06, A07, A09, A10

These categories are typically **not direct results** of dependency vulnerabilities but may be relevant in extended risk assessments:

- **A01: Broken Access Control** – applies when deps influence ACL logic indirectly. Also the correct category for SSRF-related dependency risks.
- **A06: Insecure Design** – architectural patterns enabling risk exposure through dependency choices.
- **A07: Authentication Failures** – dependencies enabling weak authentication flows or session management.
- **A09: Security Logging and Alerting Failures** – dependencies that suppress or inadequately handle audit logs. CWE-778 belongs here.
- **A10: Mishandling of Exceptional Conditions** – dependencies that expose errors insecurely or fail open. New in OWASP 2025.

---

## Notes for Agents

- **Prefer A03** for dependency-origin risks — when a dependency itself is the source (outdated, unmaintained, or vulnerable), default to A03.
- **Avoid CWE-1035 for mapping** — it is in the official A03 list but is a Category-level CWE; MITRE discourages it for vulnerability mapping. Use CWE-1395 instead.
- **CWE-310 is prohibited** per MITRE. Use CWE-327 or its specific descendants for cryptographic algorithm weaknesses.
- **CWE-16** is broad but active and officially mapped to A02:2025. Prefer specific descendants when available.
- **Use specific CWEs** to refine findings; avoid overly broad CWEs unless no specific descendant applies.