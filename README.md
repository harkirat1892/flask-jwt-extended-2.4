Just a  Fork of Flask-JWT-Extended to fix an issue with Blacklist enabled, and using SimpleKV to save tokens.

@jwt_required now gives an "is_invalid" arg to next function, so creating a decorator which gets called just below @jwt_required is the solution:

def jwt_required_fix(f):
    @wraps(f)
    def decorated_function(*argc, **kwargs):
        if "is_invalid" in kwargs:
            print("key is invalid: ", kwargs["is_invalid"])
            return handle_error_response("Invalid token")
        return f(*argc, **kwargs)
    return decorated_function
