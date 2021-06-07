# shorter-lara 
a curated list for laravel tricks & tips to write a shorter & cleaner code with laravel built-in features . 

> check the Javascript Version of this file [here](https://github.com/adnane-ka/shorter-js) .

##### Laravel Eloquent 
* Customize model table names 
* Increments & decrements 
* First or create
* Find multiple entries
* WhereProperty
* Replicate a row !
* WhereNull & whereNotNull 
* Insert & get ID !
* Multiple conditions !
* WhereDate , WhereYear , WhereMonth , WhereDay , WhereTime
* Get a Table's Column Names !
* Check if a property has been changed before !
* Get All Record but not null ones !
* More convenient DD
* Prevent Updating !
* Retrieve Random Rows
* Get only rows that have child rows !

##### Blade
* loops varriables
* Easy drop downs with laravel eloquent
* Easy & Cleaner HTML Codes With Laravel HTML Generator !
* Use forelse loops instead of foreach
* Blade directives that you did not know about
* Add Parameters to Pagination Links

##### Query Builder  
* enable query Logging & display SQL statement

##### Routing 
* Provide your own "404" procedure
* Create A Custom Route File

##### Validation 
* new password validation rules with laravel 8.39!
* Use this instead of request->all()

##### Other 
* UUID instead of auto increment !
* Missing the old SQL way?
* Request: has any

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

### WhereDate , WhereYear , WhereMonth , WhereDay , WhereTime 
```php 
$usersOfThisYear = WhereYear('created_at' ,today()->year)->get();
$usersOfThisMonth = WhereYear('created_at' ,today()->month)->get();
$usersOfThisDay = WhereYear('created_at' ,today()->day)->get();
```

### Get a Table's Column Names !
```php 
class YourModel extends Eloquent {
    
    public function getTableColumns() {
        return $this->getConnection()
        ->getSchemaBuilder()
        ->getColumnListing($this->getTable());
    }
    
}
```

### Check if a property has been changed before ! 
```php 
@if($user->isDirty('email'))
<h2> The user email was changed before ! </h2>
@endif
```

### Get All Record but not null ones !
```php 
$products = Products::where('is_available is null')->get();
```

### More convenient DD
```php 
// Instead of
$users = User::where('name', 'Taylor')->get();
dd($users);
// you can 
$users = User::where('name', 'Taylor')->get()->dd();
```

### Prevent Updating !
```php 
class Post extends Eloquent
{
	protected static function boot()
	{
		parent::boot();

		static::updating(function($model)
		{
			return false;
		});
	}
}
```

### Retrieve Random Rows
```php 
$users = User::orderByRaw('RAND()')->take(10)->get();
```

### Get only rows that have child rows !
```php 
$categories = Category::with('products')->has('products')->get();
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

### Easy & Cleaner HTML Codes With Laravel HTML Generator !
```php 
# Generating a link to external Javascript
{{ HTML::script('js/app.min.js'); }}

# Generating a link to external stylesheet
{{ HTML::style('css/style.css'); }}

# Generating image tag
{{ HTML::image('img/img1.jpg'); }}

# Generating link tag
{{ HTML::link('http://github.com','Lara-shorter', ['id'=>'myLink']); }}

# Generating obsufscated mailto tag
{{ HTML::mailto('myemail@mail.com','Some person', ['id'=>'myEmail']); }}

# Generating HTML ordered list
{{ HTML::ol(['list item', 'list item', 'list item']); }}

# Generating HTML unordered list
{{ HTML::ul(['list item', 'list item', 'list item']); }}

# Generating HTML unordered list with nested elements
{{ HTML::ul(['list item', 'list item' => ['list item','list item'], 'list item']); }}
```

### Use forelse loops instead of foreach
```php 
@forelse ($items as $item)

This is item {{ $item->id }}

@empty

No items found.

@endforelse
```

### Blade directives that you did not know about

```php 
# @json
    <script>
    let users = @json($users); // convert a collection to json 
    </script>

# @stack & @push 

    // in parent view
    @stack('scripts') 
    
    // in child view 
    @push('scripts')
    <script>
    alert('Hey Im a stacked Script!');
    </script>
    @endpush

# @inject 

    @inject('metrics' ,'App\Services\MetricServices')

    {{ $metrics->someMethod() }}

# @includeWhen 
    @includeWhen($someCondition ,'path\to\someview')
```
### Add Parameters to Pagination Links
```php 
{{ $users->appends(['sort' => 'votes'])->links() }}

{{ $users->withQueryString()->links() }}

{{ $users->fragment('foo')->links() }}
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



---
## Other  

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
### UUID instead of auto increment !
```php 
Schema::create('users', function (Blueprint $table) {
    $table->uuid('id')->unique();
});
```
### Missing the old SQL way?
```php 
DB::statement('DROP TABLE users');
```