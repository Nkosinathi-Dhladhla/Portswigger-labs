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
