```
DROP SCHEMA public CASCADE;
CREATE SCHEMA public;
```
If you are using PostgreSQL 9.3 or greater, you may also need to restore the default grants.

```
GRANT ALL ON SCHEMA public TO postgres;
GRANT ALL ON SCHEMA public TO public;
```

ref: https://stackoverflow.com/questions/3327312/how-can-i-drop-all-the-tables-in-a-postgresql-database

## Restore database
 
 `psql -U <USERNAME> -d <DBNAME> < <FILE>.sql`