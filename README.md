# lara-shorter 
a curated list for laravel tricks & tips to write a shorter & cleaner code with laravel built-in features . 

##### Laravel Eloquent 
* customize model table names 
* increments & decrements 
* first or create
* find multiple entries
* whereProperty
* replicate a row !
* whereNull & whereNotNull 
* insert & get ID !
* Multiple conditions !
* WhereDate , WhereYear , WhereMonth , WhereDay , WhereTime

##### Blade
* loops varriables
* Easy drop downs with laravel eloquent

##### Query Builder  
* enable query Logging & display SQL statement

##### Routing 
* provide your own "404" procedure

##### Validation 
* new password validation rules with laravel 8.39!


---
## Laravel Eloquent 

### customize model table names 
```php 
class User extends Model
{
    protected $table = 'users_table';
}
```

### increments & decrements 
instead of :
```php 
$article = Article::find($article_id);
$article->read_count++;
$article->save();
```
you can :
```php 
$article = Article::find($article_id);
$article->increment('read_count');
```

### first or create 
instead of : 
```php 
$user = User::where('email', $email)->first();
if (!$user) {
  User::create([
    'email' => $email
  ]);
}
```
you can : 
```php 
$user = User::firstOrCreate(['email' => $email]);
```

### find multiple entries 
```php 
User::find([1,2,4]);
```

### whereProperty 
instead of :
```php 
User::where('approved' ,true)->get()
```
you can : 
```php 
User::whereApproved(true);
```

### replicate a row !
```php 
$task = Tasks::find(1);
$newTask = $task->replicate();
$newTask->save();
```

### whereNull & whereNotNull
instead of :
```php
$article = Article::where('tags' ,null)->get(); 
$article = Article::where('tags' ,'!=',null)->get(); 
```
you can : 
```php 
$article = Article::whereNull('tags')->get();
$article = Article::whereNotNull('tags')->get(); 
```
### insert & get ID !
```php 
User::insertGetId(['name' ,'jhon doe']);
```

--- 
### Multiple conditions !
```php 
public function productsOfToday()
{
    return User::where([
      ['role' ,'=' ,'admin'],
      ['created_at' ,'=', today()]
    ])->get();
}
```

---

### WhereDate , WhereYear , WhereMonth , WhereDay , WhereTime 
```php 
$usersOfThisYear = WhereYear('created_at' ,today()->year)->get();
$usersOfThisMonth = WhereYear('created_at' ,today()->month)->get();
$usersOfThisDay = WhereYear('created_at' ,today()->day)->get();
```



---
## Blade










### Better way to create dropdown menus !
```php 
# controller
$categories = Category::lists('title', 'id');

# somewhere in your view 
{{ Form::select('category', $categories) }}
```

### loops varriables
```php 
@foreach($items as $item)
<tr>
  <td> a unique number per this row is : {{$loop->iteration}} </td>
  <td> @if($loop->first) thats the first item @endif </td>
  <td> @if($loop->last) thats the last item @endif </td>
</tr>  
@endforeach
```

---
## Query Builder




### enable query Logging & display SQL statement
```php 
DB::enableQueryLog();

User::where('name' ,'lara-shorter')->get()

dd(DB::getQueryLog()); 
```



---
## Routing 

### provide your own "404" procedure
```php 
Route::fallback(function(){
  // 
});
```

---
## Validation 

### new password validation rules with laravel 8.39!
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



