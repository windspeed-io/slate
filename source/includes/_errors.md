# Errors

The Windspeed API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is invalid. Possibly from an invalid request body or parameter(s).
401 | Unauthorized -- Your API key is invalid.
403 | Forbidden -- The requested resource is hidden for administrators only.
404 | Not Found -- The requested resource could not be found.
405 | Method Not Allowed -- You tried to access a resource with an invalid HTTP method.
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
