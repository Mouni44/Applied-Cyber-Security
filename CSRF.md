# Cross-Site Request Forgery


## What is CSRF?

Cross-site request forgery (also known as CSRF) is a web security vulnerability that allows an attacker to induce users to perform actions that they do not intend to perform. It allows an attacker to partly circumvent the same origin policy, which is designed to prevent different websites from interfering with each other.


## What is the impact of a CSRF attack? and How does CSRF work?

CSRF, or Cross-Site Request Forgery, is a type of cyber attack where an attacker tricks a user's browser into making an unintentional request to a web application on which the user is authenticated. The impact of a successful CSRF attack can range from unauthorized changes to user account settings, such as email or password, to performing privileged actions on behalf of the victim user, potentially gaining full control of the user's account and associated data.

For a CSRF attack to work, three conditions must be met:

1. Relevant Action: There must be a specific action within the targeted application that the attacker wants to induce, such as changing account settings or initiating a funds transfer.

2. Cookie-Based Session Handling: The application relies solely on session cookies to identify users, with no additional mechanisms for session validation. This means the user's browser automatically includes session cookies in requests.

3. No Unpredictable Request Parameters: The requests performing the action do not contain parameters with values unknown to the attacker. For instance, if changing a password, the attacker must know the existing password value.

In a typical scenario, the attacker constructs a web page with a hidden form that mimics the targeted action, such as changing the email address. When a victim visits this page, their browser unknowingly sends a request to the vulnerable website, utilizing the victim's authenticated session. The website processes the request as if it came from the legitimate user, allowing the attacker to carry out the intended action.

To prevent CSRF attacks, developers often use techniques like anti-CSRF tokens or implement SameSite cookie attributes to make sure that cookies are not sent with cross-site requests. These measures help validate the legitimacy of requests and protect against unauthorized actions initiated by attackers.

## How to construct a CSRF attack ?

To construct a CSRF (Cross-Site Request Forgery) attack using Burp Suite Professional:

1. Select a Request: Choose the HTTP request within Burp Suite Professional that you want to exploit or test.

2. Generate CSRF PoC: Right-click on the selected request and go to "Engagement tools / Generate CSRF PoC" in the context menu.

3. HTML Generation: Burp Suite will create HTML code that triggers the chosen request, excluding cookies (which will be added automatically by the victim's browser).

4. Tweak Options (if necessary): Modify various options in the CSRF PoC generator if needed, especially in cases where the request has unique features.

5. Copy and Paste HTML: Copy the generated HTML code and paste it into a web page. Open this page in a browser that is logged in to the vulnerable website.

6. Test the Attack: Check whether the intended request is successfully issued, and if the desired action occurs on the vulnerable website. This helps verify the effectiveness of the CSRF attack.

Using the CSRF PoC generator in Burp Suite Professional simplifies the process of creating HTML for CSRF exploits, making it easier to test and demonstrate the vulnerability.


## How to deliver a CSRF exploit ?

To deliver a CSRF (Cross-Site Request Forgery) exploit, attackers typically follow these steps:

1. Host Malicious HTML: The attacker creates a malicious HTML containing the CSRF payload, placing it on a website they control.

2. Induce Victim Visit: The attacker then tricks or induces victims into visiting their malicious website. This can be achieved by sharing a link via email, social media, or by embedding the malicious code on a popular website where users are likely to visit.

3. Delivery Mechanisms: The delivery mechanisms for CSRF are similar to those for reflected XSS (Cross-Site Scripting). The attacker may leverage links, messages, or comments on popular websites to spread the malicious content.

4. Self-Contained CSRF: In some cases, if the CSRF exploit uses the GET method and the attack can be self-contained with a single URL on the vulnerable website, the attacker may not need an external site. They can directly provide victims with a malicious URL on the vulnerable domain.

    Example:

    ```css
    <img src="https://vulnerable-website.com/email/change?email=pwned@evil-user.net">
    ```

    In this example, if changing the email address can be achieved using the GET method, the attacker can craft a self-contained attack URL that, when accessed by the victim, triggers the CSRF attack on the vulnerable website.

It's important to note that CSRF attacks rely on the trust established between a user and a website, exploiting the fact that the website cannot distinguish between legitimate and malicious requests initiated by the user's browser. To mitigate CSRF vulnerabilities, developers often implement security measures such as anti-CSRF tokens or SameSite cookie attributes.

## Common defences against CSRF

Common defenses against CSRF (Cross-Site Request Forgery) include:

1. CSRF Tokens: CSRF tokens are unique, secret values generated by the server-side application and shared with the client. When performing sensitive actions, such as submitting a form, the client must include the correct CSRF token in the request. This makes it difficult for attackers to construct a valid request on behalf of the victim without the correct token.

2. SameSite Cookies: SameSite is a browser security mechanism that determines when a website's cookies are included in requests originating from other websites. SameSite restrictions, especially Lax SameSite, prevent an attacker from triggering sensitive actions cross-site, as these actions typically require an authenticated session cookie.

3. Referer-Based Validation: Some applications use the HTTP Referer header to defend against CSRF attacks by verifying that the request originated from the application's own domain. However, this method is generally considered less effective than CSRF token validation.

These defenses aim to prevent unauthorized actions by ensuring that requests are legitimate and initiated by the user. CSRF tokens and SameSite cookies add an extra layer of validation to ensure that requests are genuine, while Referer-based validation relies on checking the source of the request. Understanding these defenses is crucial for both developers and security professionals to implement and assess the security of web applications.
