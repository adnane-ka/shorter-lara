# lara-shorter 
a curated list for laravel tricks & tips to write a shorter & cleaner code with laravel built-in features . 

---
### customize model table names 
```php 
class User extends Model
{
    protected $table = 'users_table';
}
```
---
### Easy drop downs with laravel eloquent 
```php 
# controller
$categories = Category::lists('title', 'id');

# somewhere in your view 
{{ Form::select('category', $categories) }}
```
---
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
---
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
---
### find multiple entries 
```php 
User::find([1,2,4]);
```
---
### whereProperty 
instead of :
```php 
User::where('approved' ,true)->get()
```
you can : 
```php 
User::whereApproved(true);
```
---
### replicate a row !
```php 
$task = Tasks::find(1);
$newTask = $task->replicate();
$newTask->save();
```
---
### whereNull ?
instead of :
```php
$article = Article::where('tags' ,null)->get(); 
```
you can : 
```php 
$article = Article::whereNull('tags')->get();
```
---