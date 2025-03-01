# Cross-site scripting

Cross-site scripting (XSS) is a web security vulnerability that occurs when a web application allows the injection of malicious scripts into web pages that are then viewed by other users. The primary goal of an XSS attack is to execute malicious code in the context of a victim's browser, enabling the attacker to steal sensitive information, manipulate user sessions, or perform actions on behalf of the victim without their consent.

# How XSS works:

1. Vulnerable Web Application: A web application is considered vulnerable to XSS if it fails to properly validate or sanitize user input before incorporating it into web pages. This input can include data from various sources, such as form fields, URL parameters, cookies, or even database entries.

2. Injection of Malicious Code: The attacker exploits the vulnerability by injecting malicious scripts (usually written in JavaScript) into the web application. This injected code becomes part of the content served to other users.
    - Stored XSS: The injected script is permanently stored on the target server (e.g., in a database). When other users access the affected page, they unwittingly execute the malicious code.

    - Reflected XSS: The injected script is reflected off a web server, often as part of a URL or in input fields. When a user clicks on a malicious link or submits a form, the script is executed in their browser.

3. Execution in Victim's Browser: When a user visits a page containing the injected malicious script, the script executes within their browser. Since web browsers typically trust the content from the same origin (the same website), the malicious code runs with the same privileges as the legitimate scripts on the page.

4. Compromised Interaction: The attacker can now manipulate the victim's session, steal their credentials, or perform actions on the web application on behalf of the victim. If the victim has elevated privileges, the attacker may gain control over the entire application.

The key danger lies in the ability of the attacker to run scripts in the context of a user's browser, effectively bypassing the security mechanisms designed to prevent unauthorized access to data or actions. To prevent XSS, web developers should implement proper input validation and output encoding techniques to ensure that user input is treated as data, not executable code. Additionally, using security mechanisms like Content Security Policy (CSP) can help mitigate the impact of XSS attacks. Regular security audits and code reviews are crucial to identifying and addressing potential XSS vulnerabilities in web applications.

# XSS proof of concept

In the context of testing for XSS vulnerabilities, a proof of concept typically involves injecting a payload into a web application to confirm the existence of the vulnerability. The commonly used payload is the alert() function in JavaScript because it is short, harmless, and easily noticeable when executed in the browser.

However, a change in Chrome from version 92 onward (July 20th, 2021) restricted cross-origin iframes from calling alert(). This impacted some advanced XSS attacks that rely on cross-origin iframes. To address this, an alternative payload using the print() function is recommended, as it works in situations where alert() may not.

In simulated victim scenarios where Chrome is used, labs have been adjusted to accept both alert() and print() functions for validating XSS vulnerabilities. This change ensures that users can still effectively demonstrate and understand XSS issues even with the browser restriction on alert() in cross-origin iframes.


# What are the types of XSS attacks?

## Reflected XSS (Cross-Site Scripting):
- Description: In a reflected XSS attack, the malicious script is included in the URL or a form input, and it is immediately reflected back in the response. The script is not permanently stored on the target server; instead, it is included in the response to the current HTTP request.
- Example: An attacker might craft a malicious link containing a script that steals user cookies. When a user clicks on the link, the script executes in their browser, potentially compromising their session.

## Stored XSS (Cross-Site Scripting):
- Description: In a stored XSS attack, the malicious script is permanently stored on the target server. This often occurs when user input is not properly validated or sanitized before being stored in a database. The script is then served to other users when they access the affected page or resource.
- Example: An attacker injects a script into a comment on a website. When other users view the comment section, the script executes in their browsers, allowing the attacker to steal sensitive information or perform actions on behalf of the victim.

## DOM-based XSS (Cross-Site Scripting):
- Description: Unlike reflected and stored XSS, DOM-based XSS does not necessarily involve server-side vulnerabilities. Instead, the vulnerability resides in the client-side code (Document Object Model - DOM). The attack occurs when the client-side script processes data in an unsafe way, leading to unexpected manipulation of the DOM.
- Example: An attacker might manipulate the URL or form input to trigger a client-side script that modifies the DOM, leading to the execution of malicious actions within the victim's browser.


# Reflected cross-site scripting

Reflected Cross-Site Scripting (XSS) is a type of XSS attack where the malicious script is immediately reflected back to the user in the application's response. This vulnerability occurs when the application takes data from an HTTP request and includes it in the response without proper validation or sanitization.

Here's a breakdown of a simple example illustrating a reflected XSS vulnerability:

## Normal Request-Response:

- User visits the URL https://insecure-website.com/status?message=All+is+well..
- The application processes the request and includes the value of the "message" parameter in the response: <p>Status: All is well.</p>.

## Exploiting the Vulnerability:

- An attacker recognizes the lack of proper validation in how the application handles the "message" parameter.
- The attacker constructs a malicious URL: https://insecure-website.com/status?message=<script>/* Bad stuff here... */</script>.
- The crafted URL includes a script tag with malicious JavaScript code.

## Response with Malicious Script:

- When the user visits the crafted URL, the application includes the attacker's script in the response: <p>Status: <script>/* Bad stuff here... */</script></p>.
- The script now executes in the user's browser, running in the context of the user's session with the application.

## Impact of the Attack:

- The executed script can perform any action or retrieve any data accessible to the user within the application.
- This might include stealing user credentials, session tokens, or performing unauthorized actions on behalf of the user.


# Stored cross-site scripting

Stored Cross-Site Scripting (XSS), also known as persistent or second-order XSS, occurs when an application takes data from an untrusted source and incorporates that data into its subsequent HTTP responses in an unsafe manner. This type of vulnerability arises when user-supplied data, such as comments on a blog post or user nicknames in a chat room, is stored by the application and later displayed to other users without proper validation or sanitization.

Here's a Simple example illustrating a stored XSS vulnerability in a message board application:

1. Normal User Interaction:

- Users of a message board application can submit messages that are stored by the application.

2. Normal Display of Message:

- The application displays the submitted messages to other users in subsequent HTTP responses.
- For example, a legitimate message might be displayed as: <p>Hello, this is my message!</p>.

3. Exploiting the Vulnerability:

- The application doesn't perform sufficient processing or validation of the submitted messages.
- An attacker takes advantage of this by submitting a message containing a malicious script: <p><script>/* Bad stuff here... */</script></p>.
- The script is now stored by the application.

4. Attack Unleashed:

- When other users view the message board, the stored malicious script is included in the response and executed in their browsers.
- The script runs in the context of the users' sessions with the application, allowing the attacker to carry out any action or retrieve any data accessible to those users.

5. Impact of the Attack:

- The executed script can potentially steal user credentials, session tokens, or perform unauthorized actions on behalf of the users who view the malicious message.

# DOM-based cross-site scripting

DOM-based Cross-Site Scripting (DOM XSS) is a type of cross-site scripting vulnerability where an application's client-side JavaScript processes data from an untrusted source in an insecure manner, typically by directly manipulating the Document Object Model (DOM) of the web page. Unlike reflected or stored XSS, the vulnerability in DOM-based XSS resides in the client-side code rather than the server-side code.

Here's an explanation using an example:

1. Client-Side JavaScript Code:

- The application contains JavaScript code that reads a value from an input field and writes that value to an element in the HTML DOM.

    ```js
    var search = document.getElementById('search').value;
    var results = document.getElementById('results');
    results.innerHTML = 'You searched for: ' + search;
    ```
2. Normal Usage:

- In normal usage, the JavaScript code reads the value from the input field (with the id 'search') and displays it in an element (with the id 'results').

    ```html
    <!-- HTML structure -->
    <input type="text" id="search" />
    <div id="results"></div>
    ```
3. Exploiting the Vulnerability:

- If an attacker can control the value of the input field, they can craft a malicious value that includes a script to be executed when the JavaScript writes the value back to the DOM.

    ```html
    <!-- Malicious value in the input field -->
    You searched for: <img src=1 onerror='/* Bad stuff here... */'>
    ```
4. Malicious Script Execution:

- When the JavaScript processes the malicious value and writes it to the DOM, the malicious script is executed.

    ```html
    <!-- Resulting DOM after processing the malicious value -->
    <div id="results">You searched for: <img src=1 onerror='/* Bad stuff here... */'></div>
    ```
5. Impact of the Attack:

- The executed script can perform actions within the user's browser, potentially leading to the theft of sensitive information or the compromise of user sessions.

DOM-based XSS occurs when client-side JavaScript processes untrusted data and incorporates it into the DOM in an unsafe manner, allowing malicious scripts to execute in the context of the user's browser. To prevent DOM-based XSS, developers should sanitize and validate client-side input and use secure coding practices to manipulate the DOM safely.


# What can XSS be used for?

An attacker who exploits a cross-site scripting vulnerability is typically able to:

- Impersonate or masquerade as the victim user.
- Carry out any action that the user is able to perform.
- Read any data that the user is able to access.
- Capture the user's login credentials.
- Perform virtual defacement of the web site.
- Inject trojan functionality into the web site.

# Impact of XSS vulnerabilities

The actual impact of an XSS attack generally depends on the nature of the application, its functionality and data, and the status of the compromised user. For example:

- In a brochureware application, where all users are anonymous and all information is public, the impact will often be minimal.
- In an application holding sensitive data, such as banking transactions, emails, or healthcare records, the impact will usually be serious.
- If the compromised user has elevated privileges within the application, then the impact will generally be critical, allowing the attacker to take full control of the vulnerable application and compromise all users and their data.


# How to find and test for XSS vulnerabilities

Finding and testing for Cross-Site Scripting (XSS) vulnerabilities involves both automated and manual approaches. Below is an explanation of the process described in your question:

## Automated Testing with Burp Suite's Web Vulnerability Scanner:

1. Burp Suite:

- Burp Suite is a popular web application security testing tool that includes a web vulnerability scanner. It automates the process of identifying common vulnerabilities, including XSS.
2. Web Vulnerability Scanner:

- Burp Suite's web vulnerability scanner systematically scans the target web application for potential security issues, including XSS vulnerabilities.
- It sends various payloads to input fields and analyzes the application's responses to detect instances where untrusted data is reflected or stored without proper validation.

## Manual Testing for Reflected and Stored XSS:
1. Unique Input Submission:

- Manually testing for reflected and stored XSS involves submitting unique input (e.g., a short alphanumeric string) into each entry point of the application.
- Entry points include places where user input is accepted, such as form fields, URL parameters, or any other user-controllable data.

2. Identify Locations of Returned Input:

- After submitting input, identify every location where the submitted data is returned in HTTP responses.
- This helps pinpoint potential XSS vulnerabilities.

3. Individual Testing for Exploitability:

- Test each identified location individually to determine if suitably crafted input can execute arbitrary JavaScript.
- This involves attempting to inject malicious scripts and observing the application's behavior.

## Manual Testing for DOM-based XSS:
1. Unique Input in Parameters:

- For URL-based DOM-based XSS, place unique input in the URL parameters.
2. Use Browser Developer Tools:

- Utilize the browser's developer tools to inspect the Document Object Model (DOM) for the injected input.
3. Testing for Exploitability:

- Test each location in the DOM to determine if it is exploitable by injecting malicious scripts.

## Challenges in Detecting DOM-based XSS:
- DOM-based XSS arising from non-URL parameters or non-HTML-based sinks (like document.cookie or setTimeout) can be challenging to detect automatically.
- Reviewing JavaScript code becomes necessary for these scenarios, which can be time-consuming.

## Burp Suite's Role in DOM-based XSS Detection:
- Burp Suite's web vulnerability scanner combines static and dynamic analysis of JavaScript to automate the detection of DOM-based vulnerabilities.
- It assists in identifying complex DOM-based XSS scenarios that might be hard to discover through manual testing alone.

The process involves a combination of automated scanning using tools like Burp Suite and manual testing to identify and exploit XSS vulnerabilities. Manual testing includes submitting unique input, identifying vulnerable locations, and attempting to inject malicious scripts to assess the impact. For DOM-based XSS, browser developer tools and script analysis play a crucial role, with Burp Suite providing additional automation for more advanced scenarios.


# Content Security Policy (CSP):

## Definition:
Content Security Policy (CSP) is a browser security feature that helps prevent various types of attacks, primarily focusing on mitigating the impact of cross-site scripting (XSS) vulnerabilities. It works by defining and enforcing a set of rules regarding the types of content that a browser should or should not load and execute on a web page.

## Purpose:
CSP is designed to enhance web security by reducing the risk of XSS attacks. By specifying which resources are allowed, disallowed, or restricted, CSP aims to prevent the execution of malicious scripts that could be injected into a web page.

## Limitations:
While CSP is a powerful defense mechanism, it is not foolproof. In some cases, attackers may find ways to circumvent or bypass CSP, allowing them to exploit underlying vulnerabilities. Properly configuring CSP policies is essential to its effectiveness.

# Dangling Markup Injection:

## Definition:
Dangling Markup Injection is a technique used when a full cross-site scripting exploit is not possible due to input filters or other defensive mechanisms. This technique is employed to capture data cross-domain in situations where a complete XSS attack is restricted.

## Purpose:
The goal of dangling markup injection is to exploit situations where a web application includes user input in its output, but the input is sanitized in a way that prevents traditional XSS attacks. Instead of injecting a full script, the attacker may inject incomplete or "dangling" HTML or JavaScript code that interacts with existing elements on the page.

## Exploitation:
By injecting incomplete markup, an attacker can still capture sensitive information visible to other users. This could include data such as CSRF tokens, which are crucial for preventing Cross-Site Request Forgery (CSRF) attacks. With captured CSRF tokens, an attacker could perform unauthorized actions on behalf of the user.

## Prevention:
Preventing dangling markup injection involves thorough input validation and sanitization. Developers should ensure that user input is properly filtered, and any output that includes user-generated content is encoded to prevent unintended execution of scripts.

# How to prevent XSS attacks

Preventing cross-site scripting (XSS) attacks is crucial for maintaining the security of web applications. The effectiveness of prevention measures can vary depending on the complexity of the application and how it handles user-controllable data. Here's an explanation of the key measures to prevent XSS attacks:

1. Filter Input on Arrival:

- When user input is received by the application, it's important to filter the input as strictly as possible based on the expected or valid input for each specific field or context.
- Input validation ensures that the data conforms to the expected format, length, and structure, rejecting any input that deviates from these specifications.
- Input filtering helps eliminate or neutralize potentially malicious content before it reaches other parts of the application.

2. Encode Data on Output:

- Whenever user-controllable data is output in HTTP responses, it should be encoded to prevent it from being interpreted as active content.
- Encoding involves converting special characters into their HTML, URL, JavaScript, or CSS equivalents, making it safe to display the data without triggering unintended actions.
- Different encoding techniques are applied based on the context in which the data is being used (e.g., HTML, URL parameters, JavaScript strings).

3. Use Appropriate Response Headers:

- Employing proper response headers is essential to guide the behavior of browsers when interpreting content. Two crucial headers are:
    - Content-Type: Specify the type of content being served. For non-HTML responses, make sure the Content-Type header reflects the appropriate content type (e.g., text/plain or application/json).
    - X-Content-Type-Options: Use this header to prevent browsers from interpreting files as a different MIME type. Setting it to nosniff helps mitigate MIME-sniffing vulnerabilities.

4. Content Security Policy (CSP):

- CSP is a robust defense mechanism against XSS attacks. It involves defining and enforcing a set of rules that specify which types of content are allowed to be executed on a web page.
- With CSP, you can control the sources from which scripts, styles, and other resources can be loaded. It helps prevent unauthorized execution of scripts, even if an XSS vulnerability exists.
- CSP can be configured in the web server or added to the HTML page using the Content-Security-Policy header.

# Common questions about cross-site scripting

## How common are XSS vulnerabilities?

XSS vulnerabilities are very common, and they are often considered the most frequently occurring web security vulnerability. The prevalence is due to the widespread use of dynamic web applications that involve user input and the complexity of handling and validating that input securely.

## How common are XSS attacks?

Obtaining reliable data on real-world XSS attacks can be challenging. However, it's generally believed that while XSS vulnerabilities are common, the actual exploitation of these vulnerabilities might be less frequent compared to other types of attacks. The effectiveness of exploiting XSS depends on various factors, including the security measures implemented by web applications.

## Difference between XSS and CSRF:

XSS involves injecting and executing malicious JavaScript in a user's browser by manipulating a web application's output. CSRF (Cross-Site Request Forgery), on the other hand, involves tricking a victim user into performing unintended actions on a web application where they are authenticated. While both involve web security, XSS focuses on manipulating output, while CSRF involves manipulating user actions.

## Difference between XSS and SQL injection:

XSS is a client-side vulnerability where attackers inject malicious scripts into a web application to target other users. SQL injection, on the other hand, is a server-side vulnerability where attackers inject malicious SQL queries into input fields, aiming to manipulate or extract data from the application's database. The key distinction lies in the target: XSS targets users, while SQL injection targets the application's database.

## How to prevent XSS in PHP:

- In PHP, prevent XSS by:
    - Filtering inputs using a whitelist of allowed characters.
    - Use type hints or type casting to ensure the expected data type.
    - Escape outputs using htmlentities and ENT_QUOTES for HTML contexts.
    - Use JavaScript Unicode escapes for JavaScript contexts.

## How to prevent XSS in Java:

- In Java, prevent XSS by:
    - Filtering inputs with a whitelist of allowed characters.
    - Use a library like Google Guava to HTML-encode your output for HTML contexts.
    - Use JavaScript Unicode escapes for JavaScript contexts.

In summary, XSS vulnerabilities are widespread, but the actual exploitation might be less common compared to the frequency of the vulnerabilities. It's crucial to understand the differences between XSS, CSRF, and SQL injection and implement proper security practices, such as input validation and output encoding, to prevent these vulnerabilities in PHP and Java applications.
