# Union Injection

**Overview of Union Injection:**

* Union Injection is a technique used in SQL injection attacks to combine the results of two or more SELECT statements.
* It is effective when a web application is vulnerable to SQL injection, allowing attackers to retrieve data from the database.

**Identifying Vulnerability:**

* Start by testing the search parameters for SQL injection by injecting a single quote (`'`). If an error occurs, the page may be vulnerable.

**Detecting Number of Columns:**

1. **Using ORDER BY:**
   * Inject queries incrementally to determine the number of columns.
   * Example:
     * Start with `ORDER BY 1` and continue to `ORDER BY 2`, `ORDER BY 3`, etc.
     * Stop when an error occurs (e.g., `ORDER BY 5` fails, indicating there are 4 columns).
   * Payload example:
     * `' ORDER BY 1-- -` (add space after `--`).
2. **Using UNION:**
   * Attempt UNION injections with varying numbers of columns until successful.
   * Example:
     * Start with `cn' UNION SELECT 1,2,3-- -` (expect an error).
     * Try `cn' UNION SELECT 1,2,3,4-- -` (success indicates 4 columns).

**Location of Injection:**

* Not all columns may be displayed on the web page. Identify which columns are printed to determine where to inject.
* Commonly, ID fields are not displayed, so focus on other columns.
* Use numbers as placeholders to track which columns are visible.

**Testing for Data Retrieval:**

* To confirm the ability to retrieve actual data, replace a number in the UNION query with a SQL command (e.g., `@@version`).
* Example payload:
  * `cn' UNION SELECT 1,@@version,3,4-- -` (this should display the database version).
