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
