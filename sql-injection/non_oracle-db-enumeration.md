## SQLi - database enumeration (non-Oracle)

### what i learned
information_schema is a built-in meta-database that maps every table and column
you query it to find what tables exist, then what columns exist in each table

### the full enumeration workflow
1. find column count (ORDER BY or NULL probing)
2. find text column (test with 'a')
3. extract table names:
   ' UNION SELECT table_name,NULL FROM information_schema.tables WHERE table_schema='public'#
   
4. spot the interesting table (user_xyz, not pg_* system tables)

5. extract column names from that table:
   ' UNION SELECT column_name,NULL FROM information_schema.columns WHERE table_name='users_fjcgce'#
   
6. extract the actual data with those column names:
   ' UNION SELECT username_kqppzn,password_ocdeuu FROM users_fjcgce#

### database type tells you everything
- PostgreSQL: information_schema works, comment with #, pg_* prefix on system tables
- MySQL: information_schema works, comment with # or --, no prefix on system tables
- Oracle: uses all_tables/all_columns instead, needs FROM dual, use --

### key insight
SELECT * fails when you have column limit. SELECT specific_column returns all values in that column vertically.
WHERE filters which rows you want horizontally.

### result
dumped entire user table with credentials without being told table or column names
