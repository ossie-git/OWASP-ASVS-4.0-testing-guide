# 4.2.2  CSRF protection

> Verify that the application or framework enforces a strong anti-CSRF mechanism to protect authenticated functionality, and effective anti-automation or anti-CSRF protects unauthenticated functionality.

CWE 352

# Explanation

Cross Site Request Forgery is an attack that tricks a user (a browser) into executing an action (changing password, making a wire transfer) in an application in which a user is logged in. It usually is conducted by tricking a user into clicking a link, which would execute a malicious request to the web server as a victim. Take an example of a bank:

A bank makes a transfer by making a request to the web server like this:
`GET https://bank.com/transfer?toAccount=1111&amount=100`

The attacker crafts a request which would send money to his account, 2222:

`GET https://bank.com/transfer?toAccount=2222&amount=100`

And sends it as a phishing email. A victim is authenticated in a bank, receives a phishing email with a link and clicks it. Since the victim is authenticated, the bank will make a transfer from the victim's account to attacker's account. This is a simple example, but illustrates the concept well.

There are many ways to protect an application from CSRF and OWASP Cheatsheets gives a detailed overview of them. Currently the most effective solution is using CSRF tokens. CSRF tokens, called synchronizer tokens, are tokens that is generated by the server, embedded as a hidden field in every form on the webpage and sent together with a POST request with the form data. The server then compares the sent token against the generated one. CSRF tokens protects against CSRF, because the attacker would have to guess the token for the attack to work. These tokens are often built-in into many modern web frameworks. 

Another solution is using SameSite flag for session cookies, which could be used as an additional layer of defense, next to CSRF tokens.  SameSite flag prevents cookies from being sent if the origin - domain, port, protocol - is not related to the cookie. There are cases in which for a limited time it\s possible to bypass the protection. For more information check control 3.4.3

# Testing methods

## ZAP helper script

Enable script "4-2-2 CSRF tokens.py" and browse to the site in scope. Preferably use a spider or and AJAX spider. The script looks for POST forms and searches for csrf tokens.

Bear in mind that while your application may be using CSRF tokens, if the token included in the form has a name does not contain the word "token" case insensitive, the alert will be raised. CSRF tokens are usually named "csrf-token", "_token", "csrftoken" or similar and in this case using the word token as a way to detect if the synchronizer token is in use. Be sure to check the alert for false positives.

The tested application might use other CSRF protection, so the alert being raised does not mean that it does not have any CSRF protection, but only that it does not use CSRF tokens.

For information on checking SameSite cookies see control 3.4.3. 

# Control

# Resources

[https://owasp.org/www-community/attacks/csrf](https://owasp.org/www-community/attacks/csrf)

[https://www.imperva.com/learn/application-security/csrf-cross-site-request-forgery/](https://www.imperva.com/learn/application-security/csrf-cross-site-request-forgery/)

[https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.md](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.md)

[https://medium.com/@renwa/bypass-samesite-cookies-default-to-lax-and-get-csrf-343ba09b9f2b](https://medium.com/@renwa/bypass-samesite-cookies-default-to-lax-and-get-csrf-343ba09b9f2b)