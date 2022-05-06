<div dir="rtl">

[عودة](README.md)


# التوجيه - Routing 
* [مسار احتياطي](#مسار-احتياطي)
* [قم بإنشاء ملف مسارات مخصص](#قم-بإنشاء-ملف-مسارات-مخصص)
* [طريقة أفضل للتحقق من معامل مسار ما](#طريقة-أفضل-للتحقق-من-معامل-مسار-ما)
* [متحكمات الواجهات البرمجية المصدرية](#متحكمات-الواجهات-البرمجية-المصدرية)


------------------------------------------

### مسار احتياطي

<div dir="ltr">

```php 
Route::fallback(function(){
  // 
});
```

</div>

------------------------------------------


### قم بإنشاء ملف مسارات مخصص

<div dir="ltr">

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

</div>

------------------------------------------






### طريقة أفضل للتحقق من معامل مسار ما

افترض أنك تريد التأكد من أنك تتلقى فقط معرفات أعداد صحيحة يتم تمريرها من المسار.

بدلا عن
<div dir="ltr">

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

</div>

ستحتاج فقط

<div dir="ltr">

```php 
Route::get('/api/item/{id}' ,[ItemsController::class ,'show'])
->where('id', '$[0-9]+^');
```

</div>

------------------------------------------


### متحكمات الواجهات البرمجية المصدرية
بدلا عن

<div dir="ltr">

```php
Route::resource('resourceName' ,ResourceController::class)->except([
    'show',
    'create',
    'edit'
]);
```

</div>

يمكنك

<div dir="ltr">

```php
Route::apiResource('resourceName' ,ResourceController::class);
```
</div>

------------------------------------------


</div>