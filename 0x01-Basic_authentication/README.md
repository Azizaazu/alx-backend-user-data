0. Simple-basic-API
Download and start your project from this archive.zip

In this archive, you will find a simple API with one model: User. Storage of these users is done via a serialization/deserialization in files.
1. Error handler: Unauthorized
What the HTTP status code for a request unauthorized? 401 of course!

Edit api/v1/app.py:

Add a new error handler for this status code, the response must be:
a JSON: {"error": "Unauthorized"}
status code 401
you must use jsonify from Flask
For testing this new error handler, add a new endpoint in api/v1/views/index.py:

Route: GET /api/v1/unauthorized
This endpoint must raise a 401 error by using abort - Custom Error Pages
By calling abort(401), the error handler for 401 will be executed.
2. Error handler: Forbidden
What the HTTP status code for a request where the user is authenticate but not allowed to access to a resource? 403 of course!

Edit api/v1/app.py:

Add a new error handler for this status code, the response must be:
a JSON: {"error": "Forbidden"}
status code 403
you must use jsonify from Flask
For testing this new error handler, add a new endpoint in api/v1/views/index.py:

Route: GET /api/v1/forbidden
This endpoint must raise a 403 error by using abort - Custom Error Pages
By calling abort(403), the error handler for 403 will be executed.
.
.
.
11. Basic - Overload current_user - and BOOM!
Now, you have all pieces for having a complete Basic authentication.

Add the method def current_user(self, request=None) -> TypeVar('User') in the class BasicAuth that overloads Auth and retrieves the User instance for a request:

You must use authorization_header
You must use extract_base64_authorization_header
You must use decode_base64_authorization_header
You must use extract_user_credentials
You must use user_object_from_credentials
With this update, now your API is fully protected by a Basic Authentication. Enjoy!
