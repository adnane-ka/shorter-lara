<div dir="rtl">

[عودة](README.md)

# ملفات العرض - Blade

* [طريقة أحسن لإنشاء قوائم منسدلة](#طريقة-أحسن-لإنشاء-قوائم-منسدلة)
* [متغيرات الحلقات](#متغيرات-الحلقات)
* [شيفرة أنظف مع مولد الـ HTML](#شيفرة-أنظف-مع-مولد-الـ-html)
* [حلقات forelse بدلا عن foreach](#حلقات-forelse-بدلا-عن-foreach)
* [متغيرات قالب لا غنى لك عنها](#متغيرات-قالب-لا-غنى-لك-عنها)
* [اضافة معاملات مخصصة لروابط التصفيح](#اضافة-معاملات-مخصصة-لروابط-التصفيح)
* [متغيرات قالب مخصصة](#متغيرات-قالب-مخصصة)
* [متغيرات قالب جديدة تختصر الكثير](#متغيرات-قالب-جديدة-تختصر-الكثير)
* [@parent](#parent)


------------------------------------------





### طريقة أحسن لإنشاء قوائم منسدلة

<div dir="ltr">

```php 
# بملف المتحكم
$categories = Category::lists('title', 'id');

# بملف العرض
{{ Form::select('category', $categories) }}
```

</div>

------------------------------------------





### متغيرات الحلقات

<div dir="ltr">

```php 
@foreach($items as $item)
<tr>
  <td> معرف فريد يخص هذا الصف هو: : {{$loop->iteration}} </td>
  <td> @if($loop->first) هذا هو الصف الاول @endif </td>
  <td> @if($loop->last) هذا هو الصف الاخير @endif </td>
</tr>  
@endforeach
```

</div>

------------------------------------------





### شيفرة أنظف مع مولد الـ HTML

<div dir="ltr">

```php 
# توليد وسم رابط ملف جافاسكربت خارجي
{{ HTML::script('js/app.min.js'); }}

# توليد وسم رابط تنسيقات خارجي
{{ HTML::style('css/style.css'); }}

# توليد وسم صورة
{{ HTML::image('img/img1.jpg'); }}

# توليد وسم رابط فائق
{{ HTML::link('http://github.com','Lara-shorter', ['id'=>'myLink']); }}

# Generating obsufscated mailto tag
{{ HTML::mailto('myemail@mail.com','Some person', ['id'=>'myEmail']); }}

# توليد قائمة مرتبة
{{ HTML::ol(['list item', 'list item', 'list item']); }}

# توليد قائمة غير مرتبة
{{ HTML::ul(['list item', 'list item', 'list item']); }}

# توليد قائمة غير مرتبة عناصرها متداخلة
{{ HTML::ul(['list item', 'list item' => ['list item','list item'], 'list item']); }}
```

</div>

------------------------------------------



### حلقات forelse بدلا عن foreach

<div dir="ltr">

```php 
@forelse ($items as $item)

هذا هو العنصر {{ $item->id }}

@empty

لا يوجد أي عناصر

@endforelse
```

</div>

------------------------------------------



### متغيرات قالب لا غنى لك عنها

<div dir="ltr">

```php 
# @json
    <script>
    let users = @json($users); // تحويل مجموعة الى جيسون
    </script>

# @stack & @push 

    // في ملف العرض الاب
    @stack('scripts') 
    
    // في ملف العرض الابن
    @push('scripts')
    <script>
    alert('مرحبا');
    </script>
    @endpush

# @inject 

    @inject('metrics' ,'App\Services\MetricServices')

    {{ $metrics->someMethod() }}

# @includeWhen 
    @includeWhen($someCondition ,'path\to\someview')
```

</div>

------------------------------------------


### اضافة معاملات مخصصة لروابط التصفيح

<div dir="ltr">

```php 
{{ $users->appends(['sort' => 'votes'])->links() }}

{{ $users->withQueryString()->links() }}

{{ $users->fragment('foo')->links() }}
```

</div>

------------------------------------------



### متغيرات قالب مخصصة 

قم بإضافة اية متغيرات قالب مخصصة الى 
`app\providers\AppServiceProvider.php`

<div dir="ltr">

```php
public function boot()
{
    Blade::directive('myMagicDirective', function ($expression) {
        return "any functionnality can be done to: {$expression}";
    });
} 
``` 

</div>

سيمكنك استعمالها بأي ملف عرض كـ
```html
<p>  {{ @myMagicDirective(some params) }} </p>
```
------------------------------------------

### متغيرات قالب جديدة تختصر الكثير
<div dir="ltr">

```php
// @unless - laravel 9.x
@unless (Auth::check())
    لم يتم التحقق من هويتك بعد
@endunless

// @production - laravel 9.x
@production
    انت في بيئة الانتاج
@endproduction

// @env - laravel 9.x
@env('testing')
    أنت في بيئة اختبارية
@endenv

@env(['testing' ,'production'])
    أنت في بيئة اختبارية او انتاجية
@endenv

// @each - laravel 8.x
@each('products.product-item-view', $products, 'product') # بديل عن foreach include

// @disabled, @selected, @checked - laravel 9.x
<button type="submit" @disabled($product->isDisabled)>update</button>

<select name="version">
    @foreach ($product->versions as $version)
        <option value="{{ $version }}" @selected(old('version') == $version)>
            {{ $version }}
        </option>
    @endforeach
</select>

<input type="checkbox"
        name="active"
        value="active"
        @checked(old('active', $user->active)) />        
```

</div>

------------------------------------------

### @parent

<div dir="ltr">

```php

@section('modals')
    @parent

    // اضف اي محتوى تريده دون استبدال محتوى العنصر الاب
@endsection

```

</div>

------------------------------------------

</div>