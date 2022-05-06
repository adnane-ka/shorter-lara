##### Request 
* [Request: has any](#request-has-any)
* [Request get valid data](#get-validated)

--- 
## Request 



----------------
### Request: has any
```php
public function store(Request $request) 
{
    if ($request->hasAny(['api_token', 'access_token' ,'auth_token'])) {
        echo 'We have Token passed';
    } else {
        echo 'No Token parameter';
    }
} 
```

----------------
### Get validated
```php
/**
 * @param App\Http\MyCustomFormRequest $request 
*/
public function store(MyCustomFormRequest $request) 
{
    $valid_data = $request->validated();
}
```