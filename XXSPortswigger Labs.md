https://portswigger.net/web-security/all-labs#cross-site-scripting

# Lab: Reflected XSS into HTML context with nothing encoded

This lab contains a simple reflected cross-site scripting vulnerability in the search functionality.

To solve the lab, perform a cross-site scripting attack that calls the alert function.

Process :

Step-1 : Access the lab

Step-2 : New tab will open then Type  
```javascript
<script>alert(1) </script>
``` 

Step-3 : Then in a new tab it will open a modal which shows as a error.

Step-4 : Now, we successfully completed the lab.


# Lab: Stored XSS into HTML context with nothing encoded

This lab contains a stored cross-site scripting vulnerability in the comment functionality.

To solve this lab, submit a comment that calls the alert function when the blog post is viewed. 

Peocess:

Step-1 : Access the lab

Step-2 : There you see a new tab with the posts. Click on "View Post"

Step-3 : Now, we see a list of comments and also a comment section.

Step-4 : Now make a comment with an alert and give the details like Name, Email and Website then click on "Post Comment".

Step-5 : Now we observe lab is Successfully completed


# Lab: DOM XSS in document.write sink using source location.search

This lab contains a DOM-based cross-site scripting vulnerability in the search query tracking functionality. It uses the JavaScript document.write function, which writes data out to the page. The document.write function is called with data from location.search, which you can control using the website URL.

To solve this lab, perform a cross-site scripting attack that calls the alert function. 

Process :

Step-1 : Access the lab 

Step-2 : The new will tab open with the Search bar.

Step-3 : Search for the content like " abcd12 ".

Step-4 : Open the inspector bar to detect the position of the data with for abcd12.

Step-5 : Now type this = 

```html
 "><svg onload=alert(1)> 
```

Step-6 : When we search this we can see the modal with an alert 1. 

Step-7 : Now, the lab is Successfully completed.


# Lab: DOM XSS in innerHTML sink using source location.search

This lab contains a DOM-based cross-site scripting vulnerability in the search blog functionality. It uses an innerHTML assignment, which changes the HTML contents of a div element, using data from location.search.

To solve this lab, perform a cross-site scripting attack that calls the alert function. 

Process :

Step-1 : Access the lab

Step-2 : Now we can observe Blog page with some images.

Step-3 : Now type this shows the inner HTML loading vulnerabilities.
```html
<img src=1 onerror=alert(1)>
``` 

Step-4 : When we click on search bar we observe a modal with alert 1.

Step-5 : When we click on ok. Then the lab is successfully completed.


# Lab: DOM XSS in jQuery anchor href attribute sink using location.search source

This lab contains a DOM-based cross-site scripting vulnerability in the submit feedback page. It uses the jQuery library's $ selector function to find an anchor element, and changes its href attribute using data from location.search.

To solve this lab, make the "back" link alert document.cookie. 

Process :

Step-1 : Access the lab

Step-2 : We see a new page with the Blog.

Step-3 : On the right top we see a  " Home/Submit Feedback ". Click on the "Submit Feedback".

Step-4 : On the URL type 
```url
 javascript:alert(1)
``` 
 Click enter.

Step-5 : Then we observe a page with alert operation.

Step-6 : This makes us Successfully complete the lab

