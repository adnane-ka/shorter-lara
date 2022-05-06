<div dir="rtl">

[عودة](README.md)

# التحقق - Validation
* [قواعد تحقق عملية من كلمة المرور](#قواعد-تحقق-عملية-من-كلمة-المرور)


------------------------------
### قواعد تحقق عملية من كلمة المرور

<div dir="ltr">

```php 
use Illuminate\Validation\Rules\Password;

public function rules()
{
    rules = [
        'password' => [
            'confirmed',
            Password::min(8)
            ->letters() // تمتلك على الاقل حرفا
            ->mixedCase() // تمتلك على الأقل حرفا واحد صغيرا وحرفا آخر كبيرا
            ->numbers() // تمتلك على الاقل رقما واحدا
            ->symbols() // تمتلك على الاقل رمزا واحدا
            ->uncomprimised() // كلمة المرور لا تحوي فراغات
        ],
    ];
}
```


</div>


</div>