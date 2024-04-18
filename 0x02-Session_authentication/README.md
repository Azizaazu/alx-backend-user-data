0. Et moi et moi et moi!
Copy all your work of the 0x06. Basic authentication project in this new folder.

In this version, you implemented a Basic authentication for giving you access to all User endpoints:

GET /api/v1/users
POST /api/v1/users
GET /api/v1/users/<user_id>
PUT /api/v1/users/<user_id>
DELETE /api/v1/users/<user_id>
Now, you will add a new endpoint: GET /users/me to retrieve the authenticated User object.

Copy folders models and api from the previous project 0x06. Basic authentication
Please make sure all mandatory tasks of this previous project are done at 100% because this project (and the rest of this track) will be based on it.
Update @app.before_request in api/v1/app.py:
Assign the result of auth.current_user(request) to request.current_user
Update method for the route GET /api/v1/users/<user_id> in api/v1/views/users.py:
If <user_id> is equal to me and request.current_user is None: abort(404)
If <user_id> is equal to me and request.current_user is not None: return the authenticated User in a JSON response (like a normal case of GET /api/v1/users/<user_id> where <user_id> is a valid User ID)
Otherwise, keep the same behavior
1. Empty session
Create a class SessionAuth that inherits from Auth. For the moment this class will be empty. It’s the first step for creating a new authentication mechanism:

validate if everything inherits correctly without any overloading
validate the “switch” by using environment variables
Update api/v1/app.py for using SessionAuth instance for the variable auth depending of the value of the environment variable AUTH_TYPE, If AUTH_TYPE is equal to session_auth:

import SessionAuth from api.v1.auth.session_auth
create an instance of SessionAuth and assign it to the variable auth
Otherwise, keep the previous mechanism.
.
.
.
8. Logout
Update the class SessionAuth by adding a new method def destroy_session(self, request=None): that deletes the user session / logout:

If the request is equal to None, return False
If the request doesn’t contain the Session ID cookie, return False - you must use self.session_cookie(request)
If the Session ID of the request is not linked to any User ID, return False - you must use self.user_id_for_session_id(...)
Otherwise, delete in self.user_id_by_session_id the Session ID (as key of this dictionary) and return True
Update the file api/v1/views/session_auth.py, by adding a new route DELETE /api/v1/auth_session/logout:

Slash tolerant
You must use from api.v1.app import auth
You must use auth.destroy_session(request) for deleting the Session ID contains in the request as cookie:
If destroy_session returns False, abort(404)
Otherwise, return an empty JSON dictionary with the status code 200
9. Expiration?
Actually you have 2 authentication systems:

Basic authentication
Session authentication
Now you will add an expiration date to a Session ID.

Create a class SessionExpAuth that inherits from SessionAuth in the file api/v1/auth/session_exp_auth.py:

Overload def __init__(self): method:
Assign an instance attribute session_duration:
To the environment variable SESSION_DURATION casts to an integer
If this environment variable doesn’t exist or can’t be parse to an integer, assign to 0
Overload def create_session(self, user_id=None):
Create a Session ID by calling super() - super() will call the create_session() method of SessionAuth
Return None if super() can’t create a Session ID
Use this Session ID as key of the dictionary user_id_by_session_id - the value for this key must be a dictionary (called “session dictionary”):
The key user_id must be set to the variable user_id
The key created_at must be set to the current datetime - you must use datetime.now()
Return the Session ID created
Overload def user_id_for_session_id(self, session_id=None):
Return None if session_id is None
Return None if user_id_by_session_id doesn’t contain any key equals to session_id
Return the user_id key from the session dictionary if self.session_duration is equal or under 0
Return None if session dictionary doesn’t contain a key created_at
Return None if the created_at + session_duration seconds are before the current datetime. datetime - timedelta
Otherwise, return user_id from the session dictionary
Update api/v1/app.py to instantiate auth with SessionExpAuth if the environment variable AUTH_TYPE is equal to session_exp_auth.
10. Sessions in database
Since the beginning, all Session IDs are stored in memory. It means, if your application stops, all Session IDs are lost.

For avoid that, you will create a new authentication system, based on Session ID stored in database (for us, it will be in a file, like User).

Create a new model UserSession in models/user_session.py that inherits from Base:

Implement the def __init__(self, *args: list, **kwargs: dict): like in User but for these 2 attributes:
user_id: string
session_id: string
Create a new authentication class SessionDBAuth in api/v1/auth/session_db_auth.py that inherits from SessionExpAuth:

Overload def create_session(self, user_id=None): that creates and stores new instance of UserSession and returns the Session ID
Overload def user_id_for_session_id(self, session_id=None): that returns the User ID by requesting UserSession in the database based on session_id
Overload def destroy_session(self, request=None): that destroys the UserSession based on the Session ID from the request cookie
Update api/v1/app.py to instantiate auth with SessionDBAuth if the environment variable AUTH_TYPE is equal to session_db_auth.
