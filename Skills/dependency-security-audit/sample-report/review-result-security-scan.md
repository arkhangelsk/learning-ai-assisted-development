# Dependency Security Audit Report

**Date:** February 25, 2026
**Overall Status:** ❌ FAIL
**CI Threshold:** FAIL on any HIGH or CRITICAL severity in production dependencies (default)
**Scanners Used:** `npm audit`, `osv-scanner`
**Ecosystem:** Node.js (no Python files detected)
**Total Packages Scanned:** 1,531

---

## Severity Summary

| Severity | Count |
|---|---|
| 🔴 Critical | 1 |
| 🟠 High | 25 |
| 🟡 Moderate | 8 |
| 🟢 Low | 7 |
| **Total** | **41** |

---

## Critical Findings

### 1. `fast-xml-parser` — CRITICAL (CVSS 9.3)

- **Type:** Transitive dependency (via `aws-amplify` → `@aws-amplify/storage` → `@aws-sdk/core`)
- **Issues:**
  - Entity expansion DoS via unlimited DOCTYPE entity expansion — CVSS 7.5 — [GHSA-jmr7-xgp7-cmfj](https://github.com/advisories/GHSA-jmr7-xgp7-cmfj)
  - Entity encoding bypass via regex injection in DOCTYPE entity names — CVSS 9.3 — [GHSA-m7jm-9gc2-mpf2](https://github.com/advisories/GHSA-m7jm-9gc2-mpf2)
- **Why it matters:** A remote attacker can craft XML input to either exhaust server memory (DoS) or inject regex patterns into the parser. Since `aws-amplify/storage` is a direct runtime dependency, this is reachable in production.
- **OWASP:** A03:2025 Software Supply Chain Failures
- **CWE:** CWE-1395 — Dependency on Vulnerable Third-Party Component
- **Affected range:** `4.1.3 – 5.3.5`
- **Fix:** `npm update aws-amplify`

---

## High Findings

### 2. `jsonpath` — HIGH (CVSS 9.8)

- **Type:** Transitive dependency
- **Issues:**
  - Arbitrary code injection via unsafe `eval()` of JSON path expressions — CVSS 9.8 — [GHSA-87r5-mp6g-5w5j](https://github.com/advisories/GHSA-87r5-mp6g-5w5j) — CWE-94
  - Prototype pollution via insufficient object key validation — [GHSA-6c59-mwgh-r2x6](https://github.com/advisories/GHSA-6c59-mwgh-r2x6) — CWE-1321
- **Why it matters:** If any user-controlled data flows into a `jsonpath` query, attackers can execute arbitrary JavaScript in the Node.js process. Near-maximum CVSS score.
- **OWASP:** A05:2025 Injection
- **CWE:** CWE-94 — Improper Control of Generation of Code ('Code Injection')
- **Affected range:** All versions (`*`)
- **Fix:** `npm audit fix`

---

### 3. `axios` — HIGH (CVSS 7.5) — *Direct Dependency*

- **Declared version:** `^1.11.0`
- **Issues:**
  - DoS via lack of data size validation on responses — CVSS 7.5 — [GHSA-4hjh-wcwx-xvwj](https://github.com/advisories/GHSA-4hjh-wcwx-xvwj) — CWE-770
  - DoS via `__proto__` key in `mergeConfig` — CVSS 7.5 — [GHSA-43fc-jf86-j433](https://github.com/advisories/GHSA-43fc-jf86-j433) — CWE-754
- **Why it matters:** `axios` is used directly for API calls throughout the app. Both are remotely exploitable denial-of-service vectors requiring no authentication.
- **OWASP:** A03:2025 Software Supply Chain Failures
- **CWE:** CWE-1395 — Dependency on Vulnerable Third-Party Component
- **Affected range:** `1.0.0 – 1.13.4`
- **Fix:** `npm install axios@latest` (requires ≥ 1.12.0 for first CVE, ≥ 1.13.5 for second)

---

### 4. `react-router` / `react-router-dom` — HIGH (CVSS 8.2) — *Direct Dependency*

- **Declared version:** `react-router-dom@^7.7.0`
- **Issues:**
  - XSS via open redirects — CVSS 8.0 — [GHSA-2w69-qvjg-hvjx](https://github.com/advisories/GHSA-2w69-qvjg-hvjx) — CWE-79
  - XSS in `ScrollRestoration` SSR — CVSS 8.2 — [GHSA-8v8x-cx79-35w7](https://github.com/advisories/GHSA-8v8x-cx79-35w7) — CWE-79
  - XSS vulnerability — CVSS 7.6 — [GHSA-3cgp-3xvw-98x8](https://github.com/advisories/GHSA-3cgp-3xvw-98x8) — CWE-79
  - CSRF in action/server action request processing — CVSS 6.5 — [GHSA-h5cw-625j-3rxh](https://github.com/advisories/GHSA-h5cw-625j-3rxh) — CWE-346, CWE-352
  - Unexpected external redirect via untrusted paths — CVSS 6.5 — [GHSA-9jcx-v3wj-wh4m](https://github.com/advisories/GHSA-9jcx-v3wj-wh4m) — CWE-601
- **Why it matters:** Multiple XSS vectors allow attackers to inject scripts into users' browsers. The CSRF issue enables unauthorized state-changing requests originating from malicious pages.
- **OWASP:** A05:2025 Injection (XSS), A07:2025 Authentication Failures (CSRF)
- **CWE:** CWE-79 — Cross-site Scripting (primary)
- **Affected range:** `7.0.0 – 7.11.0`
- **Fix:** `npm install react-router-dom@latest` (requires ≥ 7.12.0)

---

### 5. `highcharts` — HIGH (CVSS 7.6) — *Direct Dependency*

- **Declared version:** `^8.1.2` (pinned to v8 major)
- **Issue:** XSS if the options structure is passed unfiltered user input — CVSS 7.6 — [GHSA-8j65-4pcq-xq95](https://github.com/advisories/GHSA-8j65-4pcq-xq95) — CWE-79
- **Why it matters:** If any chart data or options are populated from user-controlled sources (e.g. API responses), attackers can inject scripts executed in the browser.
- **OWASP:** A05:2025 Injection
- **CWE:** CWE-79 — Cross-site Scripting
- **Affected range:** `< 9.0.0`
- **Fix:** Major version upgrade required — `npm install highcharts@^12.5.0` (review chart API changes before upgrading)

---

### 6. `node-forge` — HIGH (CVSS 8.6)

- **Type:** Transitive dependency
- **Issues:**
  - ASN.1 unbounded recursion — [GHSA-554w-wpv2-vw27](https://github.com/advisories/GHSA-554w-wpv2-vw27) — CWE-674
  - ASN.1 validator desynchronization / interpretation conflict — CVSS 8.6 — [GHSA-5gfm-wpxj-wjgq](https://github.com/advisories/GHSA-5gfm-wpxj-wjgq) — CWE-436
  - ASN.1 OID integer truncation — [GHSA-65ch-62r8-g69g](https://github.com/advisories/GHSA-65ch-62r8-g69g) — CWE-190
- **Why it matters:** `node-forge` performs cryptographic operations. The interpretation conflict (CVSS 8.6) can allow forged certificates or TLS bypass scenarios.
- **OWASP:** A04:2025 Cryptographic Failures
- **CWE:** CWE-436 — Interpretation Conflict
- **Affected range:** `≤ 1.3.1`
- **Fix:** `npm audit fix`

---

### 7. `aws-amplify` — HIGH — *Direct Dependency*

- **Declared version:** `^6.15.5`
- **Issue:** Transitive vulnerability chain through `@aws-amplify/analytics` and `@aws-amplify/storage` (including the `fast-xml-parser` critical above)
- **OWASP:** A03:2025 Software Supply Chain Failures
- **CWE:** CWE-1395 — Dependency on Vulnerable Third-Party Component
- **Fix:** `npm update aws-amplify`

---

### 8. Build-time HIGH findings via `react-scripts`

The following HIGH findings originate from `react-scripts`, which is a **build tool** inadvertently listed in `dependencies` but is not required at runtime. These do **not** ship to the browser. However, `react-scripts` has no upstream fix available and is superseded by the existing Vite setup.

| Package | Issue | CWE | Severity |
|---|---|---|---|
| `glob` | CLI command injection via `-c/--cmd` flag | CWE-78 | HIGH |
| `minimatch` | ReDoS via repeated wildcards | CWE-1333 | HIGH |
| `nth-check` | Inefficient regex complexity (ReDoS) | CWE-1333 | HIGH |
| `qs` / `express` / `body-parser` | DoS via array limit bypass | CWE-20 | HIGH |
| `webpack` | SSRF via `buildHttp` URL bypass | CWE-918 | LOW |
| `webpack-dev-server` | Source code theft via malicious site | CWE-346, CWE-749 | MODERATE |

> **Note:** No upstream fix is available for `react-scripts` (`fixAvailable: { version: "0.0.0" }`). Since this project already uses Vite, removing `react-scripts` entirely from `dependencies` would eliminate approximately 10–12 of these findings.

---

## Moderate / Low Findings

| Package | Issue | Severity | CWE | Advisory | Fix |
|---|---|---|---|---|---|
| `ajv` | ReDoS via `$data` option (CVE-2025-69873) | Moderate | CWE-1333, CWE-400 | [GHSA-2g4f-4pwh-qvx6](https://github.com/advisories/GHSA-2g4f-4pwh-qvx6) | `npm audit fix` |
| `js-yaml` | Prototype pollution in YAML `<<` merge | Moderate | CWE-1321 | [GHSA-mh29-5h37-fv8m](https://github.com/advisories/GHSA-mh29-5h37-fv8m) | `npm audit fix` |
| `lodash` | Prototype pollution in `_.unset`/`_.omit` | Moderate | CWE-1321 | [GHSA-xxjr-mmjv-4gpg](https://github.com/advisories/GHSA-xxjr-mmjv-4gpg) | `npm audit fix` |
| `postcss` | Line return parsing error (injection) | Moderate | CWE-74, CWE-144 | [GHSA-7fh5-64p2-3v2j](https://github.com/advisories/GHSA-7fh5-64p2-3v2j) | Via `react-scripts` removal |
| `vite` | Path traversal / `server.fs` settings bypass | Moderate | CWE-22, CWE-23 | [GHSA-g4jq-h2w9-997c](https://github.com/advisories/GHSA-g4jq-h2w9-997c) | `npm install vite@^7.0.8` |
| `react-router` (CSRF) | See finding #4 above | Moderate | CWE-352 | — | Via `react-router-dom` upgrade |
| `@smithy/config-resolver` | AWS region input not validated | Low | CWE-20 | [GHSA-6475-r3vj-m8vf](https://github.com/advisories/GHSA-6475-r3vj-m8vf) | `npm audit fix` |

---

## Observations

1. **`react-scripts` in `dependencies` is the largest contributor** to the finding count (~10–12 entries). It is a build-only tool already superseded by Vite in this project. Removing it immediately would reduce the attack surface and eliminate findings with no available upstream fix.
2. **All critical and high direct-dependency findings have safe upgrade paths** available via `npm audit fix` or targeted upgrades.
3. **`jsonpath` (CVSS 9.8)** and **`react-router`** represent the highest exploitability risk in an active web app context.
4. **`fast-xml-parser` (CVSS 9.3 Critical)** is reachable via `aws-amplify`'s storage module and is fixed in recent `aws-amplify` releases.
5. **No malicious or typosquatted packages** were detected by `osv-scanner`.
6. **`highcharts@^8.1.2`** requires a major version bump to remediate — review the Highcharts v9–v12 migration guides before upgrading.

---

## Remediation Plan

| Priority | Action | Command |
|---|---|---|
| 🔴 Immediate | Upgrade `axios` | `npm install axios@latest` |
| 🔴 Immediate | Upgrade `react-router-dom` | `npm install react-router-dom@latest` |
| 🔴 Immediate | Upgrade `aws-amplify` (fixes Critical `fast-xml-parser`) | `npm update aws-amplify` |
| 🟠 Soon | Apply all auto-fixable upgrades | `npm audit fix` |
| 🟠 Soon | Upgrade `highcharts` (review breaking changes first) | `npm install highcharts@^12.5.0` |
| 🟡 Planned | Remove `react-scripts` from `dependencies` (project uses Vite) | remove from `package.json` |
| 🟡 Planned | Move `vite`, `eslint*`, `@vitejs/plugin-react` to `devDependencies` | update `package.json` |

### Quick Remediation Commands

```bash
# Step 1: Upgrade critical/high direct dependencies
npm install axios@latest react-router-dom@latest

# Step 2: Update aws-amplify to pull in patched sub-packages
npm update aws-amplify

# Step 3: Apply remaining auto-fixable upgrades
npm audit fix

# Step 4: Upgrade highcharts (breaking change — review API first)
npm install highcharts@^12.5.0

# Step 5: Verify no regressions
npm audit
```

---

## Final Recommendation

**Do not release to production until all CRITICAL and HIGH findings in direct production dependencies are resolved.**

After completing remediation, rerun the dependency security audit to confirm the status moves to ✅ PASS.

> Scanned with: `npm audit` + `osv-scanner v1.x` | OWASP Top 10: 2025 | CWE references via MITRE
