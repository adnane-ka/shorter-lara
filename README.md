# shorter-lara 
a curated list for laravel tricks & tips to write a shorter & cleaner code with laravel built-in features . 

> check the Javascript Version of this file [here](https://github.com/adnane-ka/shorter-js) .

##### Laravel Eloquent 
* [Customize model table names](#customize-model-table-names)
* [Increments and decrements](#Increments-and-decrements)
* [First or create](#first-or-create)
* [Find multiple entries](#find-multiple-entries)
* [WhereProperty & WhereNotProperty](#whereproperty--wherenotproperty)
* [Replicate a row !](#replicate-a-row-)
* [WhereNull & whereNotNull](#wherenull--wherenotnull)
* [Insert & get ID !](#insert--get-id-)
* [Multiple conditions !](#multiple-conditions-)
* [WhereDate , WhereYear , WhereMonth , WhereDay , WhereTime](#wheredate--whereyear--wheremonth--whereday--wheretime)
* [Get a Table's Column Names !](#get-a-tables-column-names-)
* [Check if a property has been changed before !](#check-if-a-property-has-been-changed-before-)
* [Get All Record but not null ones !](#get-all-record-but-not-null-ones-)
* [More convenient DD](#more-convenient-dd)
* [Prevent Updating !](#prevent-updating-)
* [Retrieve Random Rows](#retrieve-random-rows)
* [Get only rows that have child rows !](#get-only-rows-that-have-child-rows-)
* [withCount](#withcount)

##### Blade
* [Better way to create dropdown menus !](#better-way-to-create-dropdown-menus-)
* [loops varriables](#loops-varriables)
* [Easy & Cleaner HTML Codes With Laravel HTML Generator !](#easy--cleaner-html-codes-with-laravel-html-generator-)
* [Use forelse loops instead of foreach](#use-forelse-loops-instead-of-foreach)
* [Blade directives that you did not know about](#blade-directives-that-you-did-not-know-about)
* [Add Parameters to Pagination Links](#add-parameters-to-pagination-links)
* [Custom blade directives ](#custom-blade-directives)

##### Query Builder  
* [enable query Logging & display SQL statement](#enable-query-logging--display-sql-statement)

##### Routing 
* [Provide your own "404" procedure](#provide-your-own-404-procedure)
* [Create A Custom Route File](#create-a-custom-route-file)
* [Did you know About Route::where() ?](#did-you-know-about-routewhere-)
* [more convenient way for resource API](#more-convenient-way-for-resource-API)

##### Validation 
* [new password validation rules with laravel 8.39!](#new-password-validation-rules-with-laravel-839)

##### Other 
* [Request: has any](#new-password-validation-rules-with-laravel-839)
* [UUID instead of auto increment !](#uuid-instead-of-auto-increment-)
* [Missing the old SQL way?](#missing-the-old-sql-way)
* [Arrays as Collections !](#arrays-as-collections-)
* [Grouped Collections](#grouped-collections)
* [abort_if & abort_unless](#abort_if--abort_unless)

##### Testing 
* For testing you can check this [repo](https://github.com/adnane-ka/tdd-tips-in-laravel).

---
## Laravel Eloquent 

### customize model table names 
```php 
class User extends Model
{
    protected $table = 'users_table';
}
```

### Increments and decrements
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

### whereProperty & WhereNotProperty
instead of :
```php 
User::where('approved' ,true)->get()
# or
User::where('approved' ,false)->get()
```
you can : 
```php 
User::whereApproved(true);
User::whereNotApproved(true);
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

### withCount
assuming that you want to get categories & also show products count for each category
```php 
# you can simply do
$categories = Categories::withCount('products')->get();
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

### Custom blade directives 
add your custom blade directive in app\providers\AppServiceProvider.php:
```php
public function boot()
{
    Blade::directive('myMagicDirective', function ($expression) {
        return "any functionnality can be done to: {$expression}";
    });
} 
``` 
so ,in any blade file, can use it like:
```html
<p>  {{ @myMagicDirective(some params) }} </p>
```










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

### Did you know About Route::where() ?
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


### Arrays as Collections !
```php 
$users_collection = new \Illuminate\Support\Collection([
	['name' => 'jhon doe', 'email' => 'jhondoe@doe.do'],
	['name' => 'will smith', 'email' => 'willsimth@will.sm'],
]);

// or 
$users_collection = collect([
	['name' => 'jhon doe', 'email' => 'jhondoe@doe.do'],
	['name' => 'will smith', 'email' => 'willsimth@will.sm'],
]);
```

### Grouped Collections
```php 
$collection = Person::all();

$grouped = $collection->groupBy('type');
```

### abort_if & abort_unless
instead of 
```php 
if ($order) {
  abort(404);
}
elseif(!$order){
  abort(401);
}
```
you can 
```php 
abort_if($order ,404);

abort_unless($order ,401);
```