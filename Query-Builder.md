[back](README.md)

# Query Builder

* [enable query Logging & display SQL statement](#enable-query-logging--display-sql-statement)


------------------------------------------------
### enable query Logging & display SQL statement
```php 
DB::enableQueryLog();

User::where('name' ,'lara-shorter')->get()

dd(DB::getQueryLog()); 
```

