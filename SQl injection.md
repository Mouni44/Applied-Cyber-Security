# SQL Injection (Server SIde)

## What is SQL injection(SQLi)?

SQL injection (SQLi) is a type of web security vulnerability that arises when an attacker is able to manipulate or inject malicious SQL code into the queries executed by an application's database. The vulnerability occurs when user input is not properly validated or sanitized before being included in SQL statements. This allows attackers to exploit the application's database by crafting malicious input that alters the intended behavior of the SQL queries.

The primary goal of a SQL injection attack is to interfere with the normal execution of database queries, leading to unauthorized access, retrieval, modification, or deletion of data. Attackers can leverage SQL injection to access sensitive information, such as usernames, passwords, credit card details, or other confidential data. Furthermore, they may manipulate the database to make persistent changes to the application's content or behavior.

The impact of a successful SQL injection attack can extend beyond data compromise. In some cases, attackers can escalate their privileges to compromise the underlying server or back-end infrastructure, leading to more severe consequences. Additionally, SQL injection vulnerabilities can be exploited for denial-of-service attacks, causing disruption to the availability of the application or its associated services.

To prevent SQL injection, developers should adopt secure coding practices, such as using parameterized queries (prepared statements) instead of string concatenation when building SQL statements. Parameterized queries help separate user input from SQL code, reducing the risk of injection. Regular security testing and input validation are crucial for identifying and mitigating SQL injection vulnerabilities in web applications.

## What is the impact of a successful SQL injection attack?

The impact of a successful SQL injection attack can be severe, leading to various consequences that compromise the security and integrity of a web application and its associated database. Here are some key impacts:

### Unauthorized Access to Sensitive Data:

- Attackers can gain unauthorized access to sensitive information stored in the database.
- Examples of compromised data include passwords, credit card details, personal user information, and other confidential records.

### Data Breaches:

- SQL injection attacks have been a common method employed in high-profile data breaches, exposing large volumes of sensitive data.
- Breaches can result in reputational damage to the affected organization, loss of customer trust, and legal repercussions.

### Data Manipulation and Deletion:

- Attackers can modify or delete data in the database, leading to persistent and potentially destructive changes.
- Manipulation of data can impact the accuracy and reliability of information within the application.

### Escalation of Privileges:

- In some cases, successful SQL injection attacks may enable attackers to escalate their privileges within the application or database.
- Elevated privileges could lead to unauthorized administrative access, providing attackers with greater control over the system.

### Long-term Compromise:

- A successful SQL injection attack might allow the attacker to establish a persistent backdoor into the organization's systems.
- This can result in a long-term compromise, with ongoing unauthorized access and potential for further exploitation.

### Reputational Damage:

- Data breaches and security incidents resulting from SQL injection can significantly damage the reputation of the affected organization.
- Loss of customer trust, negative publicity, and a decline in business are common consequences.

### Regulatory Consequences and Fines:

- Organizations may face regulatory consequences and financial penalties for failing to protect sensitive data.
- Data protection laws and regulations may require organizations to implement security measures to safeguard user information.

### Denial-of-Service (DoS) Attacks:

- In some situations, attackers may exploit SQL injection vulnerabilities to launch denial-of-service attacks.
- These attacks can disrupt the availability of the application or its associated services, leading to downtime and user inconvenience.

Overall, the impact of a successful SQL injection attack goes beyond immediate data compromise, affecting the organization's financial well-being, regulatory compliance, and overall trustworthiness in the eyes of users and stakeholders. It underscores the importance of implementing robust security practices to mitigate the risk of SQL injection vulnerabilities.

## How to detect SQL injection vulnerabilities ?

Detecting SQL injection vulnerabilities involves systematically testing entry points in a web application to identify potential weaknesses in the handling of user input and SQL queries. Here are manual and automated approaches to detecting SQL injection vulnerabilities:

#### Manual Testing:

1. Single Quote Character Test:

  - Submit a single quote character (' or '') to the input fields and observe the application's response.
  - Look for any error messages or anomalies in the response, as these may indicate vulnerability to SQL injection.

2. SQL-Specific Syntax Tests:

  - Submit SQL-specific syntax that evaluates to the base (original) value of the entry point.
  - Modify the syntax to evaluate to a different value and observe any systematic differences in the application's responses.
  - Look for unexpected behavior, error messages, or variations in output.

3. Boolean Conditions Test:

  - Submit Boolean conditions such as OR 1=1 and OR 1=2 in the input fields.
  - Observe the application's responses for differences, especially when the conditions are true (1=1) and false (1=2).

4. Time Delay Payloads:

  - Submit payloads designed to trigger time delays when executed within a SQL query.
  - Observe the time taken for the application to respond.
  - Differences in response times may indicate a time-based SQL injection vulnerability.

5. Out-of-Band Interaction (OAST) Payloads:

  - Submit payloads designed to trigger an out-of-band network interaction when executed within a SQL query.
  - Monitor any resulting interactions, such as DNS requests, to detect potential vulnerabilities.
  - OAST payloads can help identify blind SQL injection vulnerabilities where results are not directly visible in responses.

#### Automated Testing (Using Burp Scanner):

1. Burp Scanner:
  
  - Burp Suite is a widely used web application security testing tool that includes a scanner for automating the detection of SQL injection vulnerabilities.
  - Burp Scanner sends various payloads to input fields, analyzes responses, and identifies potential SQL injection points.
  - It automates the process of testing multiple entry points and provides a report on identified vulnerabilities.

Automated tools like Burp Scanner can efficiently find common SQL injection vulnerabilities, but manual testing remains essential for thorough examination, especially for complex scenarios and variations. Combining manual and automated approaches provides a comprehensive strategy for identifying and addressing SQL injection vulnerabilities in web applications.


## SQL injection in different parts of the query

SQL injection vulnerabilities can occur at various locations within a SQL query, not just limited to the WHERE clause of a SELECT statement. Understanding these different injection points is crucial for comprehensive security testing. Here's an explanation of SQL injection in different parts of the query:

1. WHERE Clause of SELECT Queries:

  - This is the most common type of SQL injection. Attackers manipulate the conditions in the WHERE clause to change the query's logic.
  - Example: SELECT * FROM users WHERE username = 'admin' AND password = 'password'
  - Injection: admin' OR '1'='1' --

2. UPDATE Statements:

  - SQL injection can occur within the SET clause of an UPDATE statement, where values are updated, or in the WHERE clause to modify the conditions.
  - Example: UPDATE users SET password = 'newpassword' WHERE username = 'admin'
  - Injection: admin' OR '1'='1' --

3. INSERT Statements:

  - Injection in INSERT statements involves manipulating the values being inserted into the database.
  - Example: INSERT INTO users (username, password) VALUES ('newuser', 'newpassword')
  - Injection: newuser', 'maliciouspassword') --

4. SELECT Statements - Table or Column Names:

  - SQL injection can target the table or column names in a SELECT statement.
  - Example: SELECT username, password FROM users
  - Injection: users'; DROP TABLE users --

5. SELECT Statements - ORDER BY Clause:

  - Injection in the ORDER BY clause allows attackers to control the sorting of query results.
  - Example: SELECT username, email FROM users ORDER BY username ASC
  - Injection: username ASC; DROP TABLE users --



## SQL injection examples

1. Retrieving Hidden Data:

  - Scenario: Consider a web application with a query to display products based on a category.
  - Original Query: SELECT * FROM products WHERE category = 'Electronics' AND released = 1
  - Injection: Electronics' OR 1=1 --
  - Explanation: The injection modifies the query to always be true (1=1), effectively ignoring the 'released' condition. This retrieves all products, including unreleased ones.

2. Subverting Application Logic:

  - Scenario: An application uses a query to check user credentials for login.
  - Original Query: SELECT * FROM users WHERE username = 'input_user' AND password = 'input_password'
  - Injection: admin' --
  - Explanation: The injection comments out the rest of the query. If 'admin' is a valid username, the injection allows logging in as an admin without a password check.

3. UNION Attacks:

  - Scenario: An application uses a query to display products but doesn't properly validate user input.
  - Original Query: SELECT name, description FROM products WHERE category = 'Gifts'
  - Injection: ' UNION SELECT username, password FROM users --
  - Explanation: The injection adds a UNION clause, combining the results of the original query with the results of another query retrieving usernames and passwords from the 'users' table.

4. Blind SQL Injection:

  - Scenario: The application doesn't directly display query results, but you can infer information through application responses.
  - Original Query: SELECT * FROM users WHERE username = 'input_user' AND password = 'input_password'
  - Injection: admin' AND 1=1 --
  - Explanation: Even though the results aren't directly visible, the application's behavior changes if the condition is true. By observing these changes, an attacker can infer the validity of conditions.


## Retrieving hidden data

In this scenario, the example demonstrates how a SQL injection attack can be employed to retrieve hidden data from a shopping application that displays products in different categories. The application constructs SQL queries to fetch relevant product details based on user input, such as the selected category. The query includes a condition (released = 1) to filter out unreleased products.

  #### Original Query:
```sql

SELECT * FROM products WHERE category = 'Gifts' AND released = 1
```

In this query:

 - SELECT * retrieves all details from the products table.
 - "WHERE category = 'Gifts' AND released = 1" filters the results to products in the 'Gifts' category that are released.

  #### SQL Injection Attack 1:

```url

https://insecure-website.com/products?category=Gifts'--
```

This manipulates the query by injecting a single quote (') followed by two hyphens (--). In SQL, -- is a comment indicator, causing the database to treat the rest of the query as a comment. The resulting query becomes:


```sql

SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1
```
The condition AND released = 1 is effectively commented out, and all products, including unreleased ones, are displayed.

  #### SQL Injection Attack 2:

```url

https://insecure-website.com/products?category=Gifts'+OR+1=1--
```
This injects a condition using the logical OR (OR 1=1) to always evaluate to true. The resulting query becomes:

```sql

SELECT * FROM products WHERE category = 'Gifts' OR 1=1--' AND released = 1
```

As 1=1 is always true, the entire condition becomes true, and the query returns all items, regardless of the category or the release status.

  #### Warning:
  The warning emphasizes the need for caution when injecting conditions like OR 1=1 into SQL queries. While it may seem harmless, applications might use data from a single request in multiple queries. Injecting this condition into an UPDATE or DELETE statement could lead to unintended consequences, such as accidental data loss. Therefore, it's crucial to thoroughly understand the application's logic and potential impacts before conducting SQL injection testing.

## Subverting application logic

In this scenario, the example illustrates how an attacker can subvert the application's logic and gain unauthorized access by exploiting a SQL injection vulnerability in the login functionality. The application allows users to log in with a username and password, and the login credentials are verified using the following SQL query:

  #### Original Query:

```sql
SELECT * FROM users WHERE username = 'wiener' AND password = 'bluecheese'
```
The application checks whether the query returns any user details. If it does, the login is considered successful; otherwise, it is rejected.

  #### SQL Injection Attack:

```sql
Username: administrator'--
Password: (blank)
```
In this attack, the attacker submits the username administrator'-- and leaves the password field blank. The injected part of the query becomes:

```sql

SELECT * FROM users WHERE username = 'administrator'--' AND password = ''
```
  #### Explanation:

1. SQL Comment Sequence (--): The double hyphen -- is a comment indicator in SQL. Everything after -- is treated as a comment and is not executed.

2. Injected Condition: By appending --' AND password = '' to the username, the attacker effectively comments out the rest of the original query. This means that the password check (AND password = 'bluecheese') is removed from the query.

3. Resulting Query: The modified query becomes:

    ```sql
    SELECT * FROM users WHERE username = 'administrator'--' AND password = ''
    ```
    As a result, the application checks only for the existence of a user with the username 'administrator' and ignores the password check.

4. Successful Login: Since the injected query always returns the details of the user with the username 'administrator,' the attacker can successfully log in without providing a password.

This SQL injection attack allows the attacker to manipulate the login process, bypass the password check, and gain unauthorized access to the application as the targeted user ('administrator' in this case). This highlights the importance of implementing secure coding practices, such as parameterized queries, to prevent SQL injection vulnerabilities and protect against unauthorized access.

## Retrieving data from other database tables

In this scenario, the example demonstrates how an attacker can exploit a SQL injection vulnerability to retrieve data from other database tables within the application. The attacker utilizes the UNION keyword to execute an additional SELECT query and combine its results with the original query's results.

#### Original Query:

```sql
SELECT name, description FROM products WHERE category = 'Gifts'
```

This query retrieves the names and descriptions of products from the 'products' table where the category is 'Gifts'.

#### SQL Injection Attack:

```sql
Input: ' UNION SELECT username, password FROM users--
```
#### Explanation:

1. SQL Union Attack: The injected payload ' UNION SELECT username, password FROM users-- appends a new SELECT query to the original query using the UNION keyword. This allows the attacker to combine the results of the original query with the results of the new query.

2. Resulting Query: The modified query becomes:

```sql
SELECT name, description FROM products WHERE category = 'Gifts' UNION
SELECT username, password FROM users--
```
3. Combined Results: The application executes the modified query. The first part retrieves product details, and the second part retrieves usernames and passwords from the 'users' table.

4. Returned Data: The application returns a combined result set that includes the names and descriptions of products along with the retrieved usernames and passwords from the 'users' table.

This type of attack, known as a Union-based SQL injection, takes advantage of the application's vulnerability to combine data from different tables. It is crucial for developers to implement input validation and parameterized queries to prevent SQL injection vulnerabilities and protect against unauthorized access to sensitive data. Additionally, security testing and regular code reviews can help identify and address such vulnerabilities in web applications.

## Blind SQL injection vulnerabilities

Blind SQL injection vulnerabilities occur when an application is susceptible to SQL injection attacks, but it does not directly reveal the results of the injected SQL queries or any database errors in its responses. In these cases, attackers need to employ more sophisticated techniques to exploit the vulnerability and gather information from the database indirectly.

Here are three common techniques used to exploit blind SQL injection vulnerabilities:

#### Changing Query Logic:

- Attackers can alter the logic of the SQL query to induce detectable differences in the application's response. This involves injecting conditions into Boolean logic statements within the query.
- For example, an attacker might inject a new condition that always evaluates to true or false, leading to different branches in the query logic. Depending on the outcome, the application may respond differently, revealing information about the underlying database.

#### Triggering Time Delays:

- Attackers can introduce time delays in the processing of the SQL query to infer the truth of a condition based on the time it takes for the application to respond.
- By injecting a conditional statement that introduces a delay (e.g., using the SLEEP function), attackers can observe variations in response times. Longer delays indicate a true condition, while shorter delays indicate a false condition.

#### Out-of-Band Network Interaction (OAST):

- OAST involves triggering interactions with an external network to indirectly gather information from the database.
- Attackers can use techniques like DNS exfiltration, where data is embedded in DNS lookups for domains controlled by the attacker. The application's response triggers these out-of-band interactions, allowing attackers to extract data through channels other than the regular HTTP response.

Blind SQL injection attacks are generally more complex than standard SQL injection attacks, as attackers need to rely on observing indirect responses or side effects. Each of these techniques exploits different aspects of the application's behavior to extract information from the database without directly retrieving the query results.

## Second-order SQL injection

First-order SQL injection refers to a situation where an application takes user input directly from an HTTP request and incorporates it into a SQL query in an insecure manner, leading to the possibility of unauthorized access or manipulation of the database. This is a well-known and common type of vulnerability.

Now, let's delve into second-order SQL injection:

#### Storing User Input:

- In second-order SQL injection, the application receives user input from an HTTP request, but instead of immediately using it in a SQL query, the input is stored for future use. This storage usually involves placing the input into a database.

#### Safe Initial Storage:

- The critical point in second-order SQL injection is that, at the time of storage, developers might be aware of SQL injection vulnerabilities. Consequently, they take precautions and safely store the user input in the database. This initial storage process is considered secure.

#### Unsafe Retrieval and Query Construction:

- The vulnerability arises later when the application retrieves the stored data and incorporates it into a SQL query in an insecure manner. The stored data is incorrectly deemed safe by the developer, leading to a false sense of security.
- Because the data was stored securely initially, there might be a mistaken assumption that it remains safe throughout its lifecycle. However, when the data is later processed without proper validation or sanitization, it becomes vulnerable to SQL injection.

#### Developer Misjudgment:

- Second-order SQL injection often occurs due to a misjudgment by developers who mistakenly trust the stored data, assuming that since it was stored securely, it remains safe. This misconception leads to inadequate handling of the data when constructing SQL queries during subsequent stages of the application.

In summary, second-order SQL injection is characterized by a temporal separation between the storage of user input and its later use in a SQL query. The initial storage is secure, but the subsequent retrieval and processing of the stored data are done in an insecure manner, potentially leading to SQL injection vulnerabilities. It's crucial for developers to recognize that data considered safe at one point in the application flow may become unsafe if not handled appropriately in subsequent stages.

## Examining the database

After identifying a SQL injection vulnerability, the next step for an attacker is often to gather information about the database. This information is crucial for crafting targeted and effective exploitation payloads. Here are some common techniques used to obtain information about the database:

#### Querying Version Details:

- The version of the database is a critical piece of information for attackers. Different database management systems (DBMS) have unique ways of providing version details. In the example given, the query SELECT * FROM v$version is specific to Oracle databases. It retrieves version information from the v$version view. If this query successfully executes, the attacker can infer that the target database is an Oracle database. Other databases have their own methods for retrieving version information.

#### Listing Tables and Columns:

- Knowing the structure of the database, including the tables and columns, is essential for extracting valuable data. The query SELECT * FROM information_schema.tables is a generic SQL query that works on many database platforms. The information_schema is a standard schema in SQL databases that provides metadata about the database, including details about tables, columns, and other objects. This query lists all tables present in the database.

- Note that while this query is commonly supported, there might be variations in how different databases structure their schema information. Some databases may have different system tables or views to achieve a similar result.

These techniques leverage the standardized features of SQL and the specific functionalities provided by each database platform. It's important for attackers to be aware of the syntax and methods that work on the target database to successfully extract information.

In summary, once a SQL injection vulnerability is identified, an attacker may use platform-specific queries to gather information such as the database version, tables, and columns. This information is crucial for constructing more targeted and effective SQL injection payloads, allowing the attacker to exploit the vulnerability in a manner tailored to the specific database environment.


## SQL injection in different contexts

SQL injection attacks involve manipulating an application's input to execute unintended SQL queries on its underlying database. While many SQL injection attacks are traditionally performed using the query string in URLs, it's important to recognize that SQL injection can occur in various contexts, including different data formats like JSON or XML. This allows attackers to adapt their techniques and potentially bypass security measures such as Web Application Firewalls (WAFs) that may be designed to detect and block traditional SQL injection patterns.

Here's an explanation of the provided example, which demonstrates SQL injection in an XML context:

1. XML-Based SQL Injection:
  In the example, the input is in XML format, and it is used to perform a stock check in a hypothetical scenario. The XML payload is as follows:
    ```xml
    <stockCheck>
      <productId>123</productId>
      <storeId>999 &#x53;ELECT * FROM information_schema.tables</storeId>
    </stockCheck>
    ```
    The interesting part is the content within the <storeId> element. Instead of directly injecting the SQL query with the "SELECT" keyword, which might be detected by security filters, the attacker uses an XML escape sequence (&#x53;) to represent the ASCII character for 'S'. This makes the payload look benign from a security filter's perspective.

2. Server-Side Decoding:
  The XML payload is decoded server-side before being passed to the SQL interpreter. The 'S' is converted back to the letter 'S', resulting in the SQL query: SELECT * FROM information_schema.tables. This query would then be executed by the database.

3. Bypassing Security Filters:
  The technique used in this example demonstrates an attempt to obfuscate the SQL injection attack. Security measures often look for specific keywords like "SELECT" to detect and prevent SQL injection. By encoding or escaping characters, attackers aim to bypass these filters.

4. Adapting to Different Contexts:
  The example emphasizes that SQL injection attacks are not limited to query strings in URLs. They can occur wherever an application processes user input as part of a SQL query, whether it's in JSON, XML, or any other data format. Attackers need to adapt their techniques based on the specific context in which input is processed.

In summary, understanding that SQL injection can occur in various input contexts is crucial for both attackers and defenders. It allows attackers to devise creative ways to bypass security filters, and it highlights the importance of comprehensive input validation and sanitation to prevent such attacks.


## How to prevent SQL injection

The provided explanation discusses the importance of using parameterized queries, also known as prepared statements, to prevent SQL injection attacks in database interactions. Let's break down the key points:

1. Vulnerability in String Concatenation:
  - The initial code snippet is vulnerable to SQL injection because it directly concatenates user input into the SQL query:

    ```java
    String query = "SELECT * FROM products WHERE category = '"+ input + "'";
    Statement statement = connection.createStatement();
    ResultSet resultSet = statement.executeQuery(query);
    ```
    - Here, the value of the input variable is directly inserted into the SQL query string, making it susceptible to manipulation by an attacker.

2. Using Parameterized Queries (Prepared Statements):
  - To prevent SQL injection, the recommended approach is to use parameterized queries or prepared statements. In the revised code snippet:

    ```java
    PreparedStatement statement = connection.prepareStatement("SELECT * FROM products WHERE category = ?");
    statement.setString(1, input);
    ResultSet resultSet = statement.executeQuery();
    ```

    - The PreparedStatement class is employed, and a placeholder (?) is used in the SQL query for the user input. The setString method is then used to safely set the value of the placeholder. This way, the SQL query is constructed with the user input treated as a parameter rather than being directly concatenated into the query string.

3. Limitations of Parameterized Queries:
  - Parameterized queries are effective for handling untrusted input within the WHERE clause and values in INSERT or UPDATE statements. However, they cannot be used for untrusted input in other parts of the query, such as table or column names, or the ORDER BY clause.

4. Handling Untrusted Data in Other Parts of the Query:
  - For situations where untrusted data appears in other parts of the query, alternative approaches are needed. This may include whitelisting permitted input values or using different logic to achieve the required behavior.

5. Consistency in Implementation:
  - To ensure the effectiveness of parameterized queries, it is emphasized that the string used in the query must always be a hard-coded constant and should never contain variable data from any source. Decision-making about whether an item of data is trusted should not be done on a case-by-case basis. Consistency is crucial to avoid mistakes and vulnerabilities.

In summary, using parameterized queries is a fundamental practice to prevent SQL injection attacks. It provides a secure way to handle user input within SQL queries and reduces the risk of unintended manipulation by attackers. Additionally, consistent and cautious handling of untrusted data in all parts of the query is emphasized to maintain the security of database interactions.




