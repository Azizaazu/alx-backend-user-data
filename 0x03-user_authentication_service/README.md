0. User model
In this task you will create a SQLAlchemy model named User for a database table named users (by using the mapping declaration of SQLAlchemy).

The model will have the following attributes:

id, the integer primary key
email, a non-nullable string
hashed_password, a non-nullable string
session_id, a nullable string
reset_token, a nullable string
1. create user
In this task, you will complete the DB class provided below to implement the add_user method.
2. Find user
In this task you will implement the DB.find_user_by method. This method takes in arbitrary keyword arguments and returns the first row found in the users table as filtered by the method’s input arguments. No validation of input arguments required at this point.

Make sure that SQLAlchemy’s NoResultFound and InvalidRequestError are raised when no results are found, or when wrong query arguments are passed, respectively.

Warning:

NoResultFound has been moved from sqlalchemy.orm.exc to sqlalchemy.exc between the version 1.3.x and 1.4.x of SQLAchemy - please make sure you are importing it from sqlalchemy.orm.exc
.
.
.
19. Update password end-point
In this task you will implement the update_password function in the app module to respond to the PUT /reset_password route.

The request is expected to contain form data with fields "email", "reset_token" and "new_password".

Update the password. If the token is invalid, catch the exception and respond with a 403 HTTP code.
20. End-to-end integration test
Start your app. Open a new terminal window.

Create a new module called main.py. Create one function for each of the following tasks. Use the requests module to query your web server for the corresponding end-point. Use assert to validate the response’s expected status code and payload (if any) for each task.

register_user(email: str, password: str) -> None
log_in_wrong_password(email: str, password: str) -> None
log_in(email: str, password: str) -> str
profile_unlogged() -> None
profile_logged(session_id: str) -> None
log_out(session_id: str) -> None
reset_password_token(email: str) -> str
update_password(email: str, reset_token: str, new_password: str) -> None
