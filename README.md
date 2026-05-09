# Bookshop Security Audit

Security audit of a Java 21 Swing application backed by MySQL 8.
Five vulnerabilities found, documented, exploited with proof-of-concept
attacks, and fully remediated.

Full write-up: [Read the article on Medium](YOUR MEDIUM LINK HERE)

---

## The Three Versions

| Folder | Description |
|---|---|
| 01-baseline | Original application as received — untouched reference point |
| 02-vulnerable | Vulnerable build used for attack documentation |
| 03-secured | Hardened version with all five fixes applied |

---

## Vulnerabilities Found

| ID | Vulnerability | OWASP | CWE |
|---|---|---|---|
| V-001 | SQL Injection | A03:2021 | CWE-89 |
| V-002 | Plaintext Password Storage | A02:2021 | CWE-256 |
| V-003 | No Password Policy | A07:2021 | CWE-521 |
| V-004 | No Input Validation on Stock | A03:2021 | CWE-20 |
| V-005 | No Brute Force Protection | A07:2021 | CWE-307 |

---

## Fixes Applied

- V-001: PreparedStatements replacing all raw string concatenation
- V-002: BCrypt hashing at cost factor 12 (jBCrypt 0.4)
- V-003: Regex-enforced password policy (8+ chars, upper, number, special)
- V-004: Bounds validation at the DAO layer
- V-005: HashMap-based account lockout after 5 failed attempts

---

## Setup

1. Import schema.sql into MySQL 8
2. Update DB credentials in the config file
3. Open the project in Apache NetBeans 23
4. Build with Maven and run

---

## Stack

Java 21 · Java Swing · MySQL 8 · JDBC · jBCrypt 0.4 · Apache Maven · NetBeans 23

Standards: OWASP Top 10:2025 · ASVS 4.0 · NIST SSDF SP 800-218

---

MSc Cyber Security — National College of Ireland
