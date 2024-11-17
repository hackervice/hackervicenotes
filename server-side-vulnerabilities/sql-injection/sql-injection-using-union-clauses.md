# SQL Injection Using Union Clauses

SQL Union Injection is a type of attack that exploits vulnerabilities in web applications by manipulating SQL queries. It allows an attacker to combine the results of multiple SELECT statements using the SQL Union clause, enabling the extraction of sensitive data from different tables and databases. By injecting a carefully crafted UNION query into user inputs, attackers can bypass authentication and access unauthorized information. Understanding the mechanics of Union Injection is essential for both cybersecurity professionals and developers to safeguard applications against such threats.

**Overview of Union Injection:**

* Union Injection allows the execution of entire SQL queries alongside the original query.
* It utilizes the SQL Union clause to combine results from multiple SELECT statements, enabling data extraction from various tables and databases.

**Understanding the SQL Union Clause:**

* The Union clause merges results from multiple SELECT statements.
*   Example:

    ```sql
    sqlCopy CodeSELECT * FROM ports UNION SELECT * FROM ships;
    ```

    * This combines results from both the `ports` and `ships` tables.

**Key Points:**

1. **Data Types Must Match:**
   * All selected columns in the UNION must have the same data types.
2. **Equal Number of Columns:**
   * Both SELECT statements must return the same number of columns.
   *   Example of an error when columns differ:

       {% code overflow="wrap" %}
       ```sql
       sqlCopy CodeSELECT city FROM ports UNION SELECT * FROM ships;  -- Error due to different column counts
       ```
       {% endcode %}
3. **Injecting UNION Queries:**
   *   Example of injecting a UNION query:

       {% code overflow="wrap" %}
       ```sql
       sqlCopy CodeSELECT * FROM products WHERE product_id = '1' UNION SELECT username, password FROM passwords-- '
       ```
       {% endcode %}
   * This assumes the `products` table has two columns.
4. **Handling Uneven Columns:**
   * If the original query has fewer columns than needed, use "junk" data to fill in the gaps.
   *   Example:

       {% code overflow="wrap" %}
       ```sql
       sqlCopy CodeSELECT * FROM products WHERE product_id = '1' UNION SELECT username, 2 FROM passwords;
       ```
       {% endcode %}
   * Use `NULL` or numbers as junk data to match data types.
5. **Example of Filling Columns:**
   *   If the original query has four columns:

       ```sql
       sqlCopy CodeUNION SELECT username, 2, 3, 4 FROM passwords-- '
       ```
   * This will return the username in the first column, with numbers filling the others.
