[back](README.md)

# Blade

* [Better way to create dropdown menus](#better-way-to-create-dropdown-menus)
* [loops varriables](#loops-varriables)
* [Easy and cleaner HTML codes with Laravel HTML generator](#easy-cleaner-html-codes-with-laravel-html-generator)
* [Use forelse loops instead of foreach](#use-forelse-loops-instead-of-foreach)
* [Blade directives that you did not know about](#blade-directives-that-you-did-not-know-about)
* [Add Parameters to Pagination Links](#add-parameters-to-pagination-links)
* [Custom blade directives](#custom-blade-directives)

------------------------------------------





### Better way to create dropdown menus
```php 
# controller
$categories = Category::lists('title', 'id');

# somewhere in your view 
{{ Form::select('category', $categories) }}
```
------------------------------------------





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
------------------------------------------





### Easy and cleaner HTML codes with Laravel HTML generator
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
------------------------------------------



### Use forelse loops instead of foreach
```php 
@forelse ($items as $item)

This is item {{ $item->id }}

@empty

No items found.

@endforelse
```
------------------------------------------



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
------------------------------------------


### Add Parameters to Pagination Links
```php 
{{ $users->appends(['sort' => 'votes'])->links() }}

{{ $users->withQueryString()->links() }}

{{ $users->fragment('foo')->links() }}
```
------------------------------------------



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
------------------------------------------