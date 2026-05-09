# Bookshop Security Audit

Security audit of a Java 21 Swing application backed by MySQL 8. Five vulnerabilities found, documented, exploited with proof-of-concept attacks, and fully remediated.

📖 **Full write-up:** [Read the article on Medium](https://medium.com/@shreeshk08/i-was-given-a-broken-java-app-i-found-every-flaw-proved-every-attack-then-fixed-everything-7079c433e28e)

---

## The Three Versions

| Folder | Description |
|---|---|
| `01-baseline` | Original application as received — untouched reference point |
| `02-vulnerable` | Vulnerable build used for attack documentation |
| `03-secured` | Hardened version with all five fixes applied |

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

| ID | Fix |
|---|---|
| V-001 | PreparedStatements replacing all raw string concatenation |
| V-002 | BCrypt hashing at cost factor 12 via jBCrypt 0.4 |
| V-003 | Regex-enforced password policy — 8+ chars, uppercase, number, special character |
| V-004 | Bounds validation enforced at the DAO layer |
| V-005 | HashMap-based account lockout after 5 consecutive failed attempts |

---

## Prerequisites

Install the following before running the project:

| Tool | Version | Download |
|---|---|---|
| JDK | 21 | [oracle.com](https://www.oracle.com/java/technologies/downloads/#java21) |
| MySQL | 8.0+ | [dev.mysql.com](https://dev.mysql.com/downloads/mysql/) |
| MySQL Workbench | Latest | [dev.mysql.com](https://dev.mysql.com/downloads/workbench/) |
| Apache NetBeans | 23 | [netbeans.apache.org](https://netbeans.apache.org/front/main/download/) |
| Apache Maven | 3.9+ | Bundled with NetBeans — no separate install needed |

> Maven pulls in all Java dependencies automatically including jBCrypt 0.4. No manual JAR downloads required.

---

## Setup and Installation

### 1. Clone the repository

```bash
git clone https://github.com/shreeshk08/bookshop-security-audit.git
cd bookshop-security-audit
```

### 2. Set up the database

Open MySQL Workbench and connect to your local MySQL 8 instance. Open `schema.sql` and click the lightning bolt to execute it — or run:

```sql
source /path/to/bookshop-security-audit/schema.sql
```

This creates the `bookshop` database with all tables and default users pre-loaded.

### 3. Update database credentials

In each version folder open:
src/main/java/com/bookshop/db/DatabaseConnection.java
Update these lines to match your local MySQL setup:

```java
private static final String URL      = "jdbc:mysql://localhost:3306/bookshop";
private static final String USER     = "root";          // your MySQL username
private static final String PASSWORD = "yourpassword";  // your MySQL password
```

### 4. Open in NetBeans

1. Open Apache NetBeans 23
2. Go to **File > Open Project**
3. Navigate to the version you want to run — e.g. `03-secured`
4. Click **Open Project**
5. NetBeans detects the Maven project automatically

### 5. Build

Right-click the project in the left panel and select **Clean and Build**. Maven downloads all dependencies on first build — internet connection required.

### 6. Run

Right-click the project and select **Run**, or press **F6**.

---

## Default Login Accounts

| Username | Password | Role |
|---|---|---|
| admin | admin123 | Admin |
| librarian | lib456 | Librarian |
| customer | cust123 | Customer |
| guest | guest123 | Guest |

> In `03-secured`, passwords are stored as BCrypt hashes. The default accounts still work — login verifies using `BCrypt.checkpw()`.

---

## Reproducing the Attacks (02-vulnerable only)

**SQL injection login bypass**

1. Run the `02-vulnerable` version
2. Enter `admin' OR '1'='1` in the username field
3. Leave the password field empty
4. Click Login — admin access granted with no valid credentials

**Plaintext password exposure**

1. Open MySQL Workbench
2. Run: `SELECT username, password, role FROM users;`
3. Every credential is visible in plain text

---

## Stack

| Layer | Technology |
|---|---|
| Language | Java 21 |
| UI | Java Swing |
| Database | MySQL 8 |
| Connectivity | JDBC with PreparedStatements |
| Password Hashing | jBCrypt 0.4 — BCrypt cost factor 12 |
| Build Tool | Apache Maven |
| IDE | Apache NetBeans 23 |

**Standards referenced:** OWASP Top 10:2025 · ASVS 4.0 · NIST SSDF SP 800-218 · CWE

---

*MSc Cyber Security — National College of Ireland*
