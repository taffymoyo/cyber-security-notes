## CSRF - token validation depends on request method

### what i learned
CSRF can be bypassed when server validates tokens inconsistently across request methods
vulnerable because: token present but not tied to user session

### the vulnerability
server validates CSRF token on POST requests but:
1. ignores tokens on GET requests (or doesn't validate properly)
2. token is not tied to user's session (any valid token works for any user)

### how it works
- legitimate form: POST /change-email with CSRF token (validated by server)
- exploit: use POST but with ANY valid CSRF token
- server checks "is token present?" but not "does this token belong to this user?"

### why it's vulnerable
1. token isn't session-specific
   - capture a CSRF token from one user's page
   - use that same token in exploit for different user
   - server accepts it because it doesn't validate token-to-session binding

2. inconsistent validation
   - POST validates (but weakly - just checks presence)
   - GET might not validate at all
   - inconsistency creates bypass opportunity

### the exploit
```html
<form action="https://target.com/change-email" method="POST">
  <input type="hidden" name="email" value="attacker@gmail.com">
  <input type="hidden" name="csrf" value="any_valid_token">
</form>
<script>document.forms[0].submit();</script>
```

### key lesson
CSRF tokens must be:
1. present in every request
2. tied to the user's session
3. validated on all methods (GET, POST, PUT, DELETE)
4. single-use or time-limited

if any of these is missing, token bypass is possible
