# shared-jwt-service

Small utility package for extracting a user ID from an HTTP Authorization header using PyJWT (HS256).
This package exposes a single helper used by FastAPI services to validate a Bearer token and return the `sub` claim.

## Install

This package is local, but runtime dependencies are:
- PyJWT
- FastAPI

Install dependencies:
```bash
pip install PyJWT fastapi
```

## Usage

Import and call the helper with the raw Authorization header and the secret key:

```python
from jwt_service.jwt import get_user_id_from_auth_header

auth_header = "Bearer <token>"
secret_key = "your-secret"
user_id = get_user_id_from_auth_header(auth_header, secret_key)
```

Expected token:
- Signed with HS256.
- Contains `sub` claim which is returned as the user id.
- Can include `exp` for expiration (handled by PyJWT).

## Behavior / Errors

The function raises FastAPI HTTPException (401) for:
- Missing or malformed Authorization header.
- Missing `sub` claim in token payload.
- Expired token.
- Invalid token signature or content.

## Example: creating a token (for tests)

```python
import jwt
import datetime

payload = {
    "sub": "user-123",
    "exp": datetime.datetime.utcnow() + datetime.timedelta(hours=1)
}
token = jwt.encode(payload, "your-secret", algorithm="HS256")
auth_header = f"Bearer {token}"
```

## Contributing & License

Small utility; open to improvements. No license specified — add one if required.
