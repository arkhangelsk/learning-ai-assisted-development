# Accuracy Report: owasp-mapping.md

**Reviewed:** February 22, 2026  
**Source verified against:** [OWASP Top 10:2025](https://owasp.org/Top10/2025/) official pages and MITRE CWE database

---

## Summary

The OWASP Top 10:2025 category names, numbers, and ordering are **all correct**. Several narrative claims (SSRF absorbed into A01, A03 expanding the 2021 "Vulnerable and Outdated Components" scope) are also accurate. However, the document contains **multiple CWE-to-OWASP category mapping errors** and a few factual inaccuracies in guidance notes.

---

## Issues Found

### 1. CRITICAL — A03 CWE Mappings Use Wrong CWEs

**Location:** Section "A03:2025 Software Supply Chain Failures" table

| Document Maps | Correct OWASP 2025 Category             |
| ------------- | --------------------------------------- |
| CWE-829 → A03 | CWE-829 is officially mapped to **A08** |
| CWE-494 → A03 | CWE-494 is officially mapped to **A08** |

The official OWASP 2025 A03 CWEs are: CWE-477, CWE-1035, CWE-1104, CWE-1329, CWE-1357, CWE-1395.

**Recommended fixes:**

- "Known vulnerable package" → **CWE-1395** (Dependency on Vulnerable Third-Party Component)
- "Malicious or typosquatted package" → **CWE-1357** (Reliance on Insufficiently Trustworthy Component)
- "Vulnerable transitive dependency" → **CWE-1104** (Use of Unmaintained Third-Party Components) or **CWE-1395**

---

### 2. CRITICAL — A02 Uses CWE-284, Which Belongs to A01

**Location:** Section "A02:2025 Security Misconfiguration" table, row "Dependency configuration issues"

CWE-284 (Improper Access Control) is officially mapped to **A01 (Broken Access Control)** in OWASP 2025, not A02.

The official A02 mapped CWEs are configuration-specific: CWE-5, CWE-11, CWE-13, CWE-15, CWE-16, CWE-260, CWE-315, CWE-489, CWE-526, CWE-547, CWE-611, CWE-614, CWE-776, CWE-942, CWE-1004, CWE-1174.

**Recommended fix:** Use **CWE-16** (Configuration) or **CWE-15** (External Control of System or Configuration Setting) for dependency configuration issues under A02.

---

### 3. MODERATE — A02 Uses CWE-778, Which Belongs to A09

**Location:** Section "A02:2025 Security Misconfiguration" table, row "Insecure CI/CD Env settings"

CWE-778 (Insufficient Logging) is not in the official A02 mapped CWEs. This CWE is a logging deficiency and belongs under **A09 (Security Logging and Alerting Failures)**. The finding description ("Misconfigured pipeline settings that fail to capture security-relevant events") describes a logging issue, not a misconfiguration.

**Recommended fix:** Either reclassify this row under A09, or change the CWE to something configuration-specific (e.g., CWE-16 or CWE-489).

---

### 4. MODERATE — CWE-16 Incorrectly Called "Obsolete"

**Location:** A02 table, parenthetical note

The document states: _"CWE-16 is Obsolete and PROHIBITED for mapping"_

- **"Obsolete"** is **incorrect**: CWE-16 is **not** deprecated or obsolete in MITRE's system. It is still an active CWE entry.
- **"PROHIBITED for mapping"** is partially correct: MITRE discourages CWE-16 for granular vulnerability mapping because it is a broad Class-level entry. However, **OWASP 2025 A02 explicitly includes CWE-16 in its mapped CWEs**.

**Recommended fix:** Revise to: _"CWE-16 is a broad Class-level CWE that MITRE discourages for specific vulnerability mapping; prefer more specific CWE descendants when possible."_

---

### 5. MINOR — Example Uses CWE-295, Not in Official A04 List

**Location:** Example Impact Mapping section, "Insecure SSL defaults in HTTP client lib"

CWE-295 (Improper Certificate Validation) is **not** in the official OWASP 2025 A04 mapped CWEs. The closest official match is **CWE-296** (Improper Following of a Certificate's Chain of Trust), which IS in the A04 list. The mapping is conceptually reasonable but not officially aligned.

---

### 6. MINOR — A09 Category Name Uses "&" Instead of "and"

**Location:** OWASP Top 10:2025 Categories list, item 9

The document says: **"Security Logging & Alerting Failures"**  
The official name is: **"Security Logging and Alerting Failures"**

---

### 7. MINOR — Abbreviated CWE Names

Several CWE names are informally shortened, which could cause confusion in formal audit reports:

| Document Text                                          | Official CWE Name                                                  |
| ------------------------------------------------------ | ------------------------------------------------------------------ |
| CWE-829 "Inclusion of untrusted control functionality" | CWE-829 "Inclusion of Functionality from Untrusted Control Sphere" |
| CWE-494 "Download of code w/o integrity check"         | CWE-494 "Download of Code Without Integrity Check"                 |
| CWE-327 "Risky algo use" (first A04 row)               | CWE-327 "Use of a Broken or Risky Cryptographic Algorithm"         |

---

## Confirmed Correct

- All 10 OWASP Top 10:2025 category numbers and names (except A09 ampersand)
- A01 incorporating SSRF (CWE-918), previously standalone in 2021 as A10
- A03 expanding the scope of the 2021 "Vulnerable and Outdated Components" category
- CWE-310 "PROHIBITED category" guidance is accurate per MITRE
- A05 CWE-77 and CWE-89 mappings are correct (both in official A05 list)
- A04 CWE-327 mapping is correct (in official A04 list)
- A08 CWE-494 and CWE-829 mappings are correct (both in official A08 list)
- CWE-1321 (Prototype Pollution) name and concept are accurate
- lodash@4.17.20 having prototype pollution vulnerability is plausible (fixed in 4.17.21)
