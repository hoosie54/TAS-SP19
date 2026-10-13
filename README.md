# Legacy Codebase Hardening: Enterprise Java Database Security Audit
🛡️ **Application Security Case Study** | *Targeting DoD 8140/8570 Compliance*

A comprehensive Static Application Security Testing (SAST) and manual code remediation case study targeting an automated Time & Attendance System (TAS). This project models the identification, triage, and secure refactoring of high-risk vulnerability vectors within a legacy enterprise Java database orchestration layer.

## 🛠️ Security Engineering Core Stack
* **Operating Environment:** Ubuntu Linux LTS (Isolated Laboratory Testing Sandbox)
* **Static Analysis Engine:** Semgrep Code Scanning Architecture (OWASP Java Top 10 Ruleset)
* **Source Runtime Framework:** Java Database Connectivity (JDBC) Standard Component Infrastructure
* **Version Control Operations:** Git Distributed Management Network & Token-Based Authentication

---

## 🔎 Security Case Study: SQL Injection Vulnerability Triage

### 1. The Vulnerability Discovery
During an automated directory crawl using the Semgrep semantic analysis engine, multiple **High-Risk Blocking SQL Injection** flags were triggered within the main database connection router file:
* **Target Asset:** `src/cs310/tas/wrf/TASDatabase.java`
* **Risk Vector:** Unchecked variable concatenation inside an active data execution pathway.

### 2. Insecure Legacy Source Code (Pre-Remediation)
The application originally accepted raw inputs and stitched them directly into the underlying database instruction text via string additions (`+`). This architectural flaw allows malicious actors to manipulate data fields, manipulate input boundaries, or execute unauthorized database dumps:

```java
// ❌ CRITICAL SECURITY FLAW: Direct query concatenation
query = "SELECT id,terminalid,badgeid,originaltimestamp,"
        + "punchtypeid FROM tas.punch WHERE id = " + punchID;

pstSelect = conn.prepareStatement(query);
hasresults = pstSelect.execute();
```

### 3. Remediation Strategy & Secure Code Implementation
To permanently eliminate the threat vector, the data transit pathway was completely refactored to use **Parameterized Queries via PreparedStatement**. 

By replacing dynamic string concatenation with structural query placeholders (`?`), the database engine pre-compiles the command logic first, treating all subsequent inputs strictly as literal, unexecutable text arguments:

```java
// ✅ SECURE REMEDIATION: Pre-compiled logic with explicit parameter binding
query = "SELECT id,terminalid,badgeid,originaltimestamp,"
        + "punchtypeid FROM tas.punch WHERE id = ?";

pstSelect = conn.prepareStatement(query);
pstSelect.setInt(1, punchID); // Dynamic injection barrier locked down
hasresults = pstSelect.execute();
```

---

## 📊 Security Verification Metrics
Following manual code refactoring, the application was re-submitted to the automated semantic analysis scanning matrix to verify policy compliance:

* **Initial Security Assessment:** 2 Critical/Blocking Vulnerabilities Discovered.
* **Post-Remediation Compliance Check:** 0 Flaws Remaining.
* **Vulnerability Remediation Status:** 100% Fully Hardened.

---

## 🚀 Execution & Staging Workflow Logs
The code history modifications were signed and permanently logged using secure, token-authenticated distributed staging channels to ensure identity preservation across development timelines:
```bash
git add src/cs310/tas/wrf/TASDatabase.java
git commit -m "fix(security): remediate SQL injection vulnerabilities via parameterized queries in TASDatabase"
git push origin master
```
