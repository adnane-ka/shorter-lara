<div dir="rtl">

[عودة](README.md)

# رابط الكائنات بالعلاقات - Laravel Eloquent
* [تخصيص جداول النماذج](#تخصيص-جداول-النماذج)
* [التزايد والتناقص](#التزايد-والتناقص)
* [جلب أول عنصر أو إنشاء مدخل جديد](#جلب-أول-عنصر-أو-إنشاء-مدخل-جديد)
* [جلب مجموعة من مدخلات](#جلب-مجموعة-من-مدخلات)
* [طريقة أقصر لتوابع Where](#طريقة-أقصر-لتوابع-Where)
* [استنساخ صف](#استنساخ-صف)
* [WhereNull و whereNotNull](#wherenull-و-wherenotnull)
* [إدراج وجلب المعرف](#إدراج-وجلب-المعرف)
* [شروط متعددة..](#شروط-متعددة)
* [WhereDate , WhereYear , WhereMonth , WhereDay , WhereTime](#wheredate--whereyear--wheremonth--whereday--wheretime)
* [جلب أسماء أعمدة جدول ما](#جلب-أسماء-أعمدة-جدول-ما)
* [التحقق من ما ان تم تحديث خاصية ما](#التحقق-من-ما-ان-تم-تحديث-خاصية-ما)
* [طريقة أقصر لاستثناء القيم الفارغة](#طريقة-أقصر-لاستثناء-القيم-الفارغة)
* [طريقة أفضل لإستعمال dd()](#طريقة-أفضل-لإستعمال-dd)
* [منع التحديث بشكل عام](#منع-التحديث-بشكل-عام)
* [جلب صفوف عشوائية](#جلب-صفوف-عشوائية)
* [جلب الصفوف التي تملك صفوفا أبناء فقط](#جلب-الصفوف-التي-تملك-صفوفا-أبناء-فقط)
* [تعداد العناصر الأبناء](#تعداد-العناصر-الأبناء)

--------------------------------------


### تخصيص جداول النماذج
<div dir="ltr">

```php 
class User extends Model
{
    protected $table = 'users_table';
}
```

</div>

--------------------------------------



### التزايد والتناقص
بدلا عن

<div dir="ltr">

```php 
$article = Article::find($article_id);
$article->read_count++;
$article->save();
```
</div>

يمكنك

<div dir="ltr">

```php 
$article = Article::find($article_id);
$article->increment('read_count');
```
</div>

--------------------------------------






### جلب أول عنصر أو إنشاء مدخل جديد 
بدلا عن

<div dir="ltr">

```php 
$user = User::where('email', $email)->first();
if (!$user) {
  User::create([
    'email' => $email
  ]);
}
```
</div>

يمكنك

<div dir="ltr">

```php 
$user = User::firstOrCreate(['email' => $email]);
```
</div>

--------------------------------------


### جلب مجموعة من مدخلات 

<div dir="ltr">

```php 
User::find([1,2,4]);
```
</div>

--------------------------------------



### طريقة أقصر لتوابع Where
بدلا عن

<div dir="ltr">

```php 
User::where('approved' ,true)->get()
# or
User::where('approved' ,false)->get()
```
</div>

يمكنك 

<div dir="ltr">

```php 
User::whereApproved(true);
User::whereNotApproved(true);
```
</div>

--------------------------------------








### استنساخ صف 

<div dir="ltr">

```php 
$task = Tasks::find(1);
$newTask = $task->replicate();
$newTask->save();
```
</div>

--------------------------------------








### WhereNull و whereNotNull
بدلا عن

<div dir="ltr">

```php
$article = Article::where('tags' ,null)->get(); 
$article = Article::where('tags' ,'!=',null)->get(); 
```
</div>


يمكنك 

<div dir="ltr">

```php 
$article = Article::whereNull('tags')->get();
$article = Article::whereNotNull('tags')->get(); 
```
</div>

--------------------------------------









### إدراج وجلب المعرف

<div dir="ltr">

```php 
User::insertGetId(['name' ,'jhon doe']);
```
</div>

--------------------------------------



### شروط متعددة..

<div dir="ltr">

```php 
public function productsOfToday()
{
    return User::where([
      ['role' ,'=' ,'admin'],
      ['created_at' ,'=', today()]
    ])->get();
}
```
</div>

--------------------------------------







### WhereDate , WhereYear , WhereMonth , WhereDay , WhereTime 

<div dir="ltr">

```php 
$usersOfThisYear = WhereYear('created_at' ,today()->year)->get();
$usersOfThisMonth = WhereYear('created_at' ,today()->month)->get();
$usersOfThisDay = WhereYear('created_at' ,today()->day)->get();
```
</div>

--------------------------------------





### جلب أسماء أعمدة جدول ما

<div dir="ltr">

```php 
class YourModel extends Eloquent {
    
    public function getTableColumns() {
        return $this->getConnection()
        ->getSchemaBuilder()
        ->getColumnListing($this->getTable());
    }
    
}
```
</div>

--------------------------------------


### التحقق من ما ان تم تحديث خاصية ما 

<div dir="ltr">

```php 
@if($user->isDirty('email'))
<h2> تم تحديث عنوان البريد الالكتروني من قبل بالفعل ! </h2>
@endif
```
</div>

--------------------------------------





### طريقة أقصر لاستثناء القيم الفارغة
<div dir="ltr">

```php 
$products = Products::where('is_available is null')->get();
```
</div>

--------------------------------------


### طريقة أفضل لإستعمال dd()

<div dir="ltr">

```php 
// بدلا عن
$users = User::where('name', 'Taylor')->get();
dd($users);
// يمكنك  
$users = User::where('name', 'Taylor')->get()->dd();
```
</div>

--------------------------------------




### منع التحديث بشكل عام

<div dir="ltr">

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
</div>

--------------------------------------




### جلب صفوف عشوائية

<div dir="ltr">



```php 
// بدلا عن
$users = User::orderByRaw('RAND()')->take(10)->get();
```
	
```php 
// يمكنك
$users = User::inRandomOrder()->take(10)->get();
```
	
</div>

--------------------------------------




### جلب الصفوف التي تملك صفوفا أبناء فقط

<div dir="ltr">

```php 
$categories = Category::with('products')->has('products')->get();
```
</div>

--------------------------------------



### تعداد العناصر الأبناء
بفرض أنك تريد مثلا جلب مجموع منتجات كل فئة ما بجانب معلومات الفئات 


<div dir="ltr">

```php 
# يمكنك  
$categories = Categories::withCount('products')->get();
```
</div>

--------------------------------------


</div>
