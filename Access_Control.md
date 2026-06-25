# Access Control

## What is Access Control
Access control is constraints on who or what is authorised to perform actions or access resources.

## Example
In a company an employee can view their own payroll information, a manager can view payrolls information for their team, and an HR administrator can view and modify all payroll records.

# Vertical privilege escalation
Vertical privilege is when a user gains access to permissions or functionality that belongs to a higher privileged user. 

## Example 
if a non-administrative user can gain access to an admin page where they can delete user accounts, then this is vertical privilege escalation.

## Unprotected functionality 
A user might be able to access the administrative functions of a website by browsing to relevant admin URL

### Example
A website might host sensitive functionality at the following URL: https://insecure-website.com/admin
This URL can be accessed by any user who has the link to the functionality. In some cases the URL might be disclosed at robots.txt file. https://insecure-website.com/robots.txt

## Unprotected functionality
In some cases the unprotected functionality is hidden or less predictable in the URL, but users might still be able to discover this URL in a number of ways.

## Example
This URL: https://insecure-website.com/administrator-panel-yb556
An attacker will not be able to guess it immediately but there could be clues in the javascript
<script>
	var isAdmin = false;
	if (isAdmin) {
		...
		var adminPanelTag = document.createElement('a');
		adminPanelTag.setAttribute('href', 'https://insecure-website.com/administrator-panel-yb556');
		adminPanelTag.innerText = 'Admin panel';
		...
	}
</script>
This script adds a link to the user's UI if they are an admin user. However, the script containing the URL is visible to all users regardless of their role.

## Parameter based access control
Some applications store user access rights in a user controllable location such as a hidden field, cookie, preset query string parameter. In other words the application makes access control decisions based on a submitted value. 

### Example
https://insecure-website.com/login/home.jsp?admin=true
https://insecure-website.com/login/home.jsp?role=1

This approach is insecure because a user can modify the value and access functionality they're not authorized to, such as administrative functions.

# Horizontal Privilege escalation
Horizontal privilege escalation occurs when a user is able to gain access to resources belonging to another user. Unlike vertical privilege escalation, the attacker does not become an administrator. Instead, they impersonate or access another user who has the same level of privileges.
Horizontal privilege escalation attacks may use similar types of exploit methods to vertical privilege escalation. For example, a user might access their own account page using the following URL:

https://insecure-website.com/myaccount?id=123
If an attacker modifies the id parameter value to that of another user, they might gain access to another user's account page, and the associated data and functions.

Note
This is an example of an insecure direct object reference (IDOR) vulnerability. This type of vulnerability arises where user-controller parameter values are used to access resources or functions directly.

Often, a horizontal privilege escalation attack can be turned into a vertical privilege escalation, by compromising a more privileged user
