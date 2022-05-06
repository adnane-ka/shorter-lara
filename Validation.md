[back](README.md)

# Validation
* [new password validation rules with laravel 8.39](#new-password-validation-rules-with-laravel-839)


------------------------------
### new password validation rules with laravel 8.39
```php 
use Illuminate\Validation\Rules\Password;

public function rules()
{
    rules = [
        'password' => [
            'confirmed',
            Password::min(8)
            ->letters() // require at least one charchter 
            ->mixedCase() // require at least one uppercase & one lower case
            ->numbers() // require at least one number 
            ->symbols() // require at least one symbol
            ->uncomprimised() // unsure that password isn't comprimissed
        ],
    ];
}
```

