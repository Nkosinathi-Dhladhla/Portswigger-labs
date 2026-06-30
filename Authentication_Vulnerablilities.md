# Authentication Vulnerabilities
Authentication Vulnerabilities enable attackers to access sensitive data and functionality in a system 

## Difference between authentication and authorization
Authentication is the process of verifying that the user is who they say they are 

### Example
A user tries to login. Then the website verifies that the person who is trying to access the account is the same person that created the account. 

## Authorization
Once the account is accessed and the person is authenticated, their permissions determine what they are authorized to do. 

## Brute-force attacks
A brute-force attack is when an attacker uses a system of trial and error to guess valid user credentials.

## User Enumeration
User enumeration is a security issue where an attacker can determine whether a username, email address, or account exists in a system.

This usually happens when an application responds differently for valid and invalid users.

### Example
Suppose a login form returns:
"User does not exist" for an invalid username
"Incorrect password" for a valid username with the wrong password
An attacker can try many usernames and identify which accounts exist based on the error message.

## Solving the LAB 
- Access the lab
- Go to the login page
- Enter random credentials
- Go to burp suite to intercept the credentials
- Inside burp suite go to the HTTP history
- Find the login interception
- Send the login interception to intruder
- Look for the login credentials in intruder
- Add payload on the username/password
- Copy the potential usernames/passwords that were given by the lab
- Paste on intruder
- Attack in the payload. If you're attacking the username the length must be different, if you're attacking the password the status code must be different
- Paste username and password in login page, and complete the lab!!

## Bypassing two-factor authentication
At times, the implementation of two-factor authentication is flawed to the point where it can be bypassed entirely.

If the user is first prompted to enter a password, and then prompted to enter a verification code on a separate page, the user is effectively in a "logged in" state before they have entered the verification code. In this case, it is worth testing to see if you can directly skip to "logged-in only" pages after completing the first authentication step. Occasionally, you will find that a website doesn't actually check whether or not you completed the second step before loading the page.


## LAB 2
To complete this lab, login using the user credentials, request the code, manually change the URL and add /my-account.
/my-account is simply a URL path (endpoint) that points to a user's account page.

#### For example, if the website is:

https://example.com

then visiting:

https://example.com/my-account

takes you to:

/my-account
Why is /my-account important in the 2FA bypass lab?

In the "Bypassing two-factor authentication" lab, the normal login flow is:

Enter username and password.
The server verifies them.
You're redirected to the 2FA verification page.
Enter the 2FA code.
Only then should you access /my-account.

The vulnerability is that the application creates a logged-in session before the 2FA check is completed. If you manually browse to:

/my-account

instead of completing the 2FA step, the server incorrectly lets you into your account.
