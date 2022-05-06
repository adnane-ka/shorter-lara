<div dir="rtl">

[عودة](README.md)


# الطلبات - Request 
* [الطلب يمتلك أيا من كذا وكذا](#الطلب-يمتلك-أيا-من-كذا-وكذا)
* [جلب البيانات المتحقق منها فقط في المتحكم](#جلب-البيانات-المتحقق-منها-فقط-في-المتحكم)


----------------
### الطلب يمتلك أيا من كذا وكذا

<div dir="ltr">

```php
public function store(Request $request) 
{
    if ($request->hasAny(['api_token', 'access_token' ,'auth_token'])) {
        echo 'نمتلك رمزا مميزا، يمكن تجديد الرمز او الوصول الى المورد';
    } else {
        echo 'لا يوجد اي رمز، اعد التوجيه الى صفحة تسجيل الدخول';
    }
} 
```

</div>

----------------
### جلب البيانات المتحقق منها فقط في المتحكم

<div dir="ltr">

```php
/**
 * @param App\Http\MyCustomFormRequest $request 
*/
public function store(MyCustomFormRequest $request) 
{
    $valid_data = $request->validated();
}
```

</div>

----------------

</div>