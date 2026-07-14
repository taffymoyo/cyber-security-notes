## CSRF - no defences

### what i learned
CSRF = Cross-Site Request Forgery
attacker's page can make requests to a target site using victim's session cookie
no CSRF token = can't verify request actually came from legitimate page vs attacker's page

### how the exploit works
1. capture the vulnerable request (email change POST)
2. create HTML form that replicates that request
3. auto-submit with JavaScript
4. victim visits attacker's `/exploit` page
5. browser sends victim's session cookie automatically
6. action happens without permission

### why it works
- no CSRF token to verify request legitimacy
- browser automatically sends cookies cross-origin
- no extra confirmation required
- victim has no idea what happened

### result
victim's email changed by attacker without permission
