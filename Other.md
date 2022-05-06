##### Other 

* [UUID instead of auto increment !](#uuid-instead-of-auto-increment-)
* [Missing the old SQL way?](#missing-the-old-sql-way)
* [Arrays as Collections !](#arrays-as-collections-)
* [Grouped Collections](#grouped-collections)
* [abort_if & abort_unless](#abort_if--abort_unless)


---
## Other  
-----------------------------------------------

### UUID instead of auto increment !
```php 
Schema::create('users', function (Blueprint $table) {
    $table->uuid('id')->unique();
});
```
-----------------------------------------------


### Missing the old SQL way?
```php 
DB::statement('DROP TABLE users');
```
-----------------------------------------------


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
-----------------------------------------------


### Grouped Collections
```php 
$collection = Person::all();

$grouped = $collection->groupBy('type');
```
-----------------------------------------------



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
-----------------------------------------------
