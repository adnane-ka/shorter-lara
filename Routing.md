##### Routing 
* [Provide your own "404" procedure](#provide-your-own-404-procedure)
* [Create A Custom Route File](#create-a-custom-route-file)
* [Did you know About Route::where()](#did-you-know-about-routewhere)
* [more convenient way for resource API](#more-convenient-way-for-resource-API)


---
## Routing 
------------------------------------------

### provide your own "404" procedure
```php 
Route::fallback(function(){
  // 
});
```
------------------------------------------


### Create A Custom Route File
```php 
# in App/Providers/RouteServiceProvider.php;
    
    /**
     * Define the "web" routes for the application.
     *
     * These routes all receive session state, CSRF protection, etc.
     *
     * @return void
     */
    protected function mapWebRoutes()
    {
        Route::middleware('web')
             ->namespace($this->namespace)
             ->group(base_path('routes/web.php'));

        Route::middleware('admin')
        ->prefix('admin')
        ->namespace($this->namespace.'\Admin')
        ->group(base_path('routes/admin.php'));
    }
```
------------------------------------------






### Did you know About Route::where()
Assume that you want to make sure that you're receving only integer ids passed from route . 

instead of 
```php 
Route::get('/api/item/{id}' ,[ItemsController::class ,'show']);

...
class ItemsController extends Controller {
    public function show($id)
    {

        if(!is_inetegr($id)){
            abort(404);
        }

        // some code

    }
}
```

you can 

```php 
Route::get('/api/item/{id}' ,[ItemsController::class ,'show'])
->where('id', '$[0-9]+^');
```
------------------------------------------


### more convenient way for resource API
instead of:
```php
Route::resource('resourceName' ,ResourceController::class)->except([
    'show',
    'create',
    'edit'
]);
```
you can:
```php
Route::apiResource('resourceName' ,ResourceController::class);
```

------------------------------------------
