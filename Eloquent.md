[back](README.md)

# Laravel Eloquent 
* [Customize model table names](#customize-model-table-names)
* [Increments and decrements](#Increments-and-decrements)
* [First or create](#first-or-create)
* [Find multiple entries](#find-multiple-entries)
* [WhereProperty and WhereNotProperty](#whereproperty-and-wherenotproperty)
* [Replicate a row](#replicate-a-row)
* [WhereNull and whereNotNull](#wherenull-and-wherenotnull)
* [Insert and get ID](#insert-and-get-id)
* [Multiple conditions](#multiple-conditions)
* [WhereDate , WhereYear , WhereMonth , WhereDay , WhereTime](#wheredate--whereyear--wheremonth--whereday--wheretime)
* [Get a Table's Column Names](#get-a-tables-column-names)
* [Check if a property has been changed before](#check-if-a-property-has-been-changed-before)
* [Get All Record but not null ones](#get-all-record-but-not-null-ones)
* [More convenient DD](#more-convenient-dd)
* [Prevent Updating](#prevent-updating)
* [Retrieve Random Rows](#retrieve-random-rows)
* [Get only rows that have child rows](#get-only-rows-that-have-child-rows)
* [withCount](#withcount)

--------------------------------------


### customize model table names 
```php 
class User extends Model
{
    protected $table = 'users_table';
}
```
--------------------------------------



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
--------------------------------------






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
--------------------------------------


### find multiple entries 
```php 
User::find([1,2,4]);
```
--------------------------------------



### WhereProperty and WhereNotProperty
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
--------------------------------------








### Replicate a row
```php 
$task = Tasks::find(1);
$newTask = $task->replicate();
$newTask->save();
```
--------------------------------------








### WhereNull and whereNotNull
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
--------------------------------------









### Insert and get ID
```php 
User::insertGetId(['name' ,'jhon doe']);
```
--------------------------------------



### Multiple conditions
```php 
public function productsOfToday()
{
    return User::where([
      ['role' ,'=' ,'admin'],
      ['created_at' ,'=', today()]
    ])->get();
}
```
--------------------------------------







### WhereDate , WhereYear , WhereMonth , WhereDay , WhereTime 
```php 
$usersOfThisYear = WhereYear('created_at' ,today()->year)->get();
$usersOfThisMonth = WhereYear('created_at' ,today()->month)->get();
$usersOfThisDay = WhereYear('created_at' ,today()->day)->get();
```
--------------------------------------





### Get a Table's Column Names
```php 
class YourModel extends Eloquent {
    
    public function getTableColumns() {
        return $this->getConnection()
        ->getSchemaBuilder()
        ->getColumnListing($this->getTable());
    }
    
}
```
--------------------------------------


### Check if a property has been changed before 
```php 
@if($user->isDirty('email'))
<h2> The user email was changed before ! </h2>
@endif
```
--------------------------------------





### Get All Record but not null ones
```php 
$products = Products::where('is_available is null')->get();
```
--------------------------------------


### More convenient DD
```php 
// Instead of
$users = User::where('name', 'Taylor')->get();
dd($users);
// you can 
$users = User::where('name', 'Taylor')->get()->dd();
```
--------------------------------------




### Prevent Updating
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
--------------------------------------




### Retrieve Random Rows
```php 
$users = User::orderByRaw('RAND()')->take(10)->get();
```
--------------------------------------




### Get only rows that have child rows
```php 
$categories = Category::with('products')->has('products')->get();
```
--------------------------------------



### withCount
assuming that you want to get categories & also show products count for each category
```php 
# you can simply do
$categories = Categories::withCount('products')->get();
```
--------------------------------------
