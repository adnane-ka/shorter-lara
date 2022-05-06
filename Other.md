<div dir="rtl">

[عودة](README.md)

# آخر - Other 

* [معرفات فريدة بدل المتصاعدة تلقائيا](#معرفات-فريدة-بدل-المتصاعدة-تلقائيا)
* [طريقة SQL القديمة؟](#طريقة-sql-القديمة؟)
* [مصفوفات كتجميعات](#مصفوفات-كتجميعات)
* [مجموعات مجمعة](#مجموعات-مجمعة)
* [abort_if و abort_unless](#abort_if-و-abort_unless)

-----------------------------------------------

### معرفات فريدة بدل المتصاعدة تلقائيا

<div dir="ltr">

```php 
Schema::create('users', function (Blueprint $table) {
    $table->uuid('id')->unique();
});
```

</div>

-----------------------------------------------


### طريقة SQL القديمة؟

<div dir="ltr">

```php 
DB::statement('DROP TABLE users');
```

</div>

-----------------------------------------------


### مصفوفات كتجميعات

<div dir="ltr">

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

</div>

-----------------------------------------------


### مجموعات مجمعة

<div dir="ltr">

```php 
$collection = Person::all();

$grouped = $collection->groupBy('type');
```

</div>

-----------------------------------------------



### abort_if و abort_unless
عوضا عن

<div dir="ltr">

```php 
if ($order) {
  abort(404);
}
elseif(!$order){
  abort(401);
}
```

</div>

يمكنك

<div dir="ltr">

```php 
abort_if($order ,404);

abort_unless($order ,401);
```

</div>

-----------------------------------------------

</div>