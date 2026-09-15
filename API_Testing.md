# API Testing 
- API's (Application Programming Interfaces) enable software systems and applications to communicate and share data.
- This document will mainly be focused on testing APIs that are not fully used by the websites frontend, this includes JSON and RESTful APIs

# API Recon
- Before testing an API you need to know as much information about the API as possible to discover its attack surface
- You must start by identifying the API endpoints. E.g: /api/users/profile. This is where the API receives a request about a specific resource on its server.
- After identifying the API endpoints you need to find a way to interact with them. You should find out about the following information:
  - The input data the API processes, including both compulsory and optional parameters.
  - The types of requests the API accepts, including supported HTTP methods and media formats.
  - Rate limits and authentication mechanisms.
- Always start the API Recon by viewing documentation. Even if API documentation isn't openly available, you may still be able to access it by browsing applications that use the API. This can be done by using burp scanner to crawl the API.
- Look for endpoints that may refer to API documentation, for example:
 - /api
 - /swagger/index.html
 - /openapi.json
- If you identify an endpoint for a resource, make sure to investigate the base path. For example, if you identify the resource endpoint /api/swagger/v1/users/123, then you should investigate the following paths:
 - /api/swagger/v1
 - /api/swagger
 - /api
You can also use a list of common paths to find documentation using Intruder.

## LAB 
To solve the lab, find the exposed API documentation and delete carlos. You can log in to your own account using the following credentials: wiener:peter.

https://youtu.be/AxzpOVS23o8?si=Y7TNZhFNzJC1wQe4

## Using machine-readable documentation
You can use a range of automated tools to analyze any machine-readable API documentation that you find.

You can use Burp Scanner to crawl and audit OpenAPI documentation, or any other documentation in JSON or YAML format. You can also parse OpenAPI documentation using the OpenAPI Parser BApp.

You may also be able to use a specialized tool to test the documented endpoints, such as Postman or SoapUI

# Identifying API endpoints
- You can use Burp Scanner to crawl the application, then manually investigate interesting attack surface using Burp's browser.
- While browsing the application, look for patterns that suggest API endpoints in the URL structure, such as /api/. Also look out for JavaScript files. These can contain references to API endpoints that you haven't triggered directly via the web browser

## Interacting with API endpoints
- Once you've identified API endpoints, interact with them using Burp Repeater and Burp Intruder. This enables you to observe the API's behavior and discover additional attack surface. For example, you could investigate how the API responds to changing the HTTP method and media type.
- As you interact with the API endpoints, review error messages and other responses closely. Sometimes these include information that you can use to construct a valid HTTP request.

### Identifying supported HTTP methods
The HTTP method specifies the action to be performed on a resource. For example:

- GET - Retrieves data from a resource.
- PATCH - Applies partial changes to a resource.
- OPTIONS - Retrieves information on the types of request methods that can be used on a resource.
An API endpoint may support different HTTP methods. It's therefore important to test all potential methods when you're investigating API endpoints. This may enable you to identify additional endpoint functionality, opening up more attack surface.

For example, the endpoint /api/tasks may support the following methods:

- GET /api/tasks - Retrieves a list of tasks.
- POST /api/tasks - Creates a new task.
- DELETE /api/tasks/1 - Deletes a task.
You can use the built-in HTTP verbs list in Burp Intruder to automatically cycle through a range of methods.

### Identifying supported content types
API endpoints often expect data in a specific format. They may therefore behave differently depending on the content type of the data provided in a request. Changing the content type may enable you to:
- Trigger errors that disclose useful information.
- Bypass flawed defenses.
- Take advantage of differences in processing logic. For example, an API may be secure when handling JSON data but susceptible to injection attacks when dealing with XML.
To change the content type, modify the Content-Type header, then reformat the request body accordingly. You can use the Content type converter BApp to automatically convert data submitted within requests between XML and JSON.

## LAB
To solve the lab, exploit a hidden API endpoint to buy a Lightweight l33t Leather Jacket. You can log in to your own account using the following credentials: wiener:peter.

https://youtu.be/UUzdFir0HFo?si=OobP284uVxKkxads

### Using Intruder to find hidden endpoints
Once you have identified some initial API endpoints, you can use Intruder to uncover hidden endpoints. For example, consider a scenario where you have identified the following API endpoint for updating user information:

PUT /api/user/update

To identify hidden endpoints, you could use Burp Intruder to find other resources with the same structure. For example, you could add a payload to the /update position of the path with a list of other common functions, such as delete and add.

When looking for hidden endpoints, use wordlists based on common API naming conventions and industry terms. Make sure you also include terms that are relevant to the application, based on your initial recon.

# Finding hidden parameters
When you're doing API recon, you may find undocumented parameters that the API supports. You can attempt to use these to change the application's behavior. Burp includes numerous tools that can help you identify hidden parameters:

- Burp Intruder enables you to automatically discover hidden parameters, using a wordlist of common parameter names to replace existing parameters or add new parameters. Make sure you also include names that are relevant to the application, based on your initial recon.
- The Param miner BApp enables you to automatically guess up to 65,536 param names per request. Param miner automatically guesses names that are relevant to the application, based on information taken from the scope.
- The Content discovery tool enables you to discover content that isn't linked from visible content that you can browse to, including parameters.

## Mass assignment vulnerabilities
- Mass assignment (also known as auto-binding) can create hidden parameters that were never intended to be processed by the developer.

### Identifying hidden parameters
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/af2a702f-b404-4ccd-a537-814dda90a72a" />

### Testing mass assignment vulnerabilities
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/cf90499b-3551-445d-9846-cc1b944380aa" />
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/349484b5-e944-4141-86e1-c424498bee6a" />

## LAB
To solve the lab, find and exploit a mass assignment vulnerability to buy a Lightweight l33t Leather Jacket. You can log in to your own account using the following credentials: wiener:peter.

https://youtu.be/HWJEG6PAprM?si=m_bZ9OEKE-40pA1h

# Preventing vulnerabilities in APIs
When designing APIs, make sure that security is a consideration from the beginning. In particular, make sure that you:

- Secure your documentation if you don't intend your API to be publicly accessible.
- Ensure your documentation is kept up to date so that legitimate testers have full visibility of the API's attack surface.
- Apply an allowlist of permitted HTTP methods.
- Validate that the content type is expected for each request or response.
- Use generic error messages to avoid giving away information that may be useful for an attacker.
- Use protective measures on all versions of your API, not just the current production version.
To prevent mass assignment vulnerabilities, allowlist the properties that can be updated by the user, and blocklist sensitive properties that shouldn't be updated by the user.
