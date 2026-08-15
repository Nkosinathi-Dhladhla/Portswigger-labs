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

## Lab 1
With Burp running, investigate the login page and submit an invalid username and password.
In Burp, go to Proxy > HTTP history and find the POST /login request. Highlight the value of the username parameter in the request and send it to Burp Intruder.
In Burp Intruder, notice that the username parameter is automatically set as a payload position. This position is indicated by two § symbols, for example: username=§invalid-username§. Leave the password as any static value for now.
Make sure that Sniper attack is selected.
In the Payloads side panel, make sure that the Simple list payload type is selected.
Under Payload configuration, paste the list of candidate usernames. Finally, click  Start attack. The attack will start in a new window.
When the attack is finished, examine the Length column in the results table. You can click on the column header to sort the results. Notice that one of the entries is longer than the others. Compare the response to this payload with the other responses. Notice that other responses contain the message Invalid username, but this response says Incorrect password. Make a note of the username in the Payload column.
Close the attack and go back to the Intruder tab. Click Clear §, then change the username parameter to the username you just identified. Add a payload position to the password parameter. The result should look something like this:

username=identified-user&password=§invalid-password§
In the Payloads side panel, clear the list of usernames and replace it with the list of candidate passwords. Click  Start attack.
When the attack is finished, look at the Status column. Notice that each request received a response with a 200 status code except for one, which got a 302 response. This suggests that the login attempt was successful - make a note of the password in the Payload column.
Log in using the username and password that you identified and access the user account page to solve the lab.

Note
It's also possible to brute-force the login using a single cluster bomb attack. However, it's generally much more efficient to enumerate a valid username first if possible.

https://youtu.be/DEUCRYGt3TY?si=EomPF4dgKT9mzj6X

## Lab 2
Log in to your own account. Your 2FA verification code will be sent to you by email. Click the Email client button to access your emails.
Go to your account page and make a note of the URL.
Log out of your account.
Log in using the victim's credentials.
When prompted for the verification code, manually change the URL to navigate to /my-account. The lab is solved when the page loads.

https://youtu.be/2WpBVanEn3M?si=DzV2FFHGIGxVpNcL
