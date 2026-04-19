## Workshop 3
### SQL Injection
**WebGoat Lessons:** A3 – Injection → SQL Injection (Intro)

---

#### What I Did

This workshop introduced SQL injection — one of the most well-known and dangerous web vulnerabilities — through a series of escalating tasks in WebGoat.

**Tasks 2–5 – Basic SQL Syntax**

The first few tasks were about understanding SQL syntax before injection. I practiced writing SELECT, UPDATE, ALTER TABLE, and GRANT statements to manipulate a sample database. These laid the groundwork for understanding how injections exploit poorly constructed queries.

**Task 6 – Tautology-Based Injection**

This task demonstrated the core mechanic of SQL injection. The backend query looked like:

```sql
SELECT * FROM users WHERE name = '';
```

By entering `Smith' OR '1' = '1` as the username, the query becomes:

```sql
SELECT * FROM users WHERE name = 'Smith' OR '1' = '1';
```

Since `'1' = '1'` always evaluates to true, the query returns all rows in the table regardless of the actual username — a complete authentication bypass.

**Task 9 – Extracting All User Data**

I injected `' or '1' = '1` into the first name field of a search form. This caused the query to return the entire contents of the `user_data` table, including user IDs, names, credit card numbers, and login counts.

**Task 10 – Numeric Field Injection**

The `User_Id` field was vulnerable. Entering `0 OR 1=1` into it caused the query to return all records, since `1=1` is always true.

**Task 11 – Bypassing Authentication via TAN Field**

The Authentication TAN field was injectable. By appending `' OR '1' = '1` to the TAN input while entering `Smith` as the employee name, I was able to retrieve the entire employee table including salaries and TANs.

**Task 12 – Query Chaining to Modify Data**

Using the `;` metacharacter to chain a second query, I modified a salary value:

```
Authentication TAN: '; UPDATE employees SET salary=88888 WHERE first_name='John
```

This demonstrated that injection can go beyond data disclosure — it can also modify stored data.

**Task 13 – Dropping a Table**

By injecting into the action field with query chaining and a comment operator to ignore the rest of the query:

```
'; DROP TABLE access_log;--
```

I was able to delete the `access_log` table entirely, showing that injection can cause destructive, irreversible changes to a database.

---

#### What I Learned

SQL injection is powerful precisely because it abuses the way applications construct queries by concatenating user input directly into SQL strings. Each task escalated the impact: from reading data, to modifying it, to deleting it entirely. The mitigation is to never concatenate user input into queries — instead, use parameterised queries (also called prepared statements), which treat user input as data rather than executable code.

---

#### OWASP Reference

- **A03:2021 – Injection**
