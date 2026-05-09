# Bookshop Security Audit

Security audit of a Java 21 Swing application backed by MySQL 8.
Five vulnerabilities found, documented, exploited with proof-of-concept
attacks, and fully remediated.

Full write-up: [Read the article on Medium](https://medium.com/@shreeshk08/i-was-given-a-broken-java-app-i-found-every-flaw-proved-every-attack-then-fixed-everything-7079c433e28e)

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

## Prerequisites

Install these before running anything:

| Tool | Version | Download |
|---|---|---|
| JDK | 21 | https://www.oracle.com/java/technologies/downloads/#java21 |
| MySQL | 8.0+ | https://dev.mysql.com/downloads/mysql/ |
| MySQL Workbench | Latest | https://dev.mysql.com/downloads/workbench/ |
| Apache NetBeans | 23 | https://netbeans.apache.org/front/main/download/ |
| Apache Maven | 3.9+ | Bundled with NetBeans — no separate install needed |

> Maven handles all Java dependencies automatically including jBCrypt 0.4.
> You do not need to download any JAR files manually.

---

## Setup

### 1. Clone the repository

```bash
git clone https://github.com/shreeshk08/bookshop-security-audit.git
cd bookshop-security-audit
```

### 2. Set up the database

Open MySQL Workbench and connect to your local MySQL 8 instance.

Run the schema file to create the database and seed it with data:

```sql
source /path/to/bookshop-security-audit/schema.sql
```

Or open `schema.sql` in MySQL Workbench and click the lightning bolt to execute it.

This creates the `bookshop` database with all tables and default users pre-loaded.

### 3. Update the database credentials

In each project version (01-baseline, 02-vulnerable, 03-secured), open the database config file:
