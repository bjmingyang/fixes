---
author: ''
category: Laravel
date: '2015-10-09'
summary: ''
title: Laravel Routes
---
# Laravel Routes

URL endpoints defined at `app/Http/routes.php`

### Using RESTful routes

It is important to dedicate each controller to an associated resources

[Simple explanation on REST](http://stackoverflow.com/questions/551933/can-you-explain-the-web-concept-of-restful)

All you need to do in laravel for _each_ controller is:

```
Route::resource('lists', 'ListController');
```

Then you would need the following actions:
`index, create, store, show, edit, update, destroy`

### Using Implicit Routes (Non REST)

A controller isn't intended for REST then you can just use:

```
Route::controllers({
    'lists' => 'ListsController'
  });
```

You would then need to ensure that the action names are prefixed with `get` or `post` so that laravel knows.

```
class ListController extends Controller {
    public function getIndex(){
      return ...
    }

    public function getCreate()...

    public function postStore()...
}
```

### Defining Route Parameters

```
Route::get('blog/category/{category}', 'BlogController@category');
```

So `{category}` would become whatever is in the url ie. `/blog/category/food`

```
$category = 'food';
```

You would need the action to be setup:

```
public function category($category){
  return view('blob.category')->with('category', $category);
}
```

**Caveat**: They need to be specified in the same order as parameters are specified

**Caveat**: parameters are _required_ in the below case

```
Route::get('blog/category/{category}/{subcategory}', 'blogController@category');

public function category($category, $subcategory){
  return view('blog.category')
    ->with('category', $category)
    ->with('category', $subcategory);
}
```

**To make parameters not required is quote a rigmeral if you ask me**

```
Route::get('blog/category/{category?}', 'BlogController@category');

public function category($category = ''){
  $category = $category == '' ? 'php : $category;'
  return view('blob.category')->with('category', $category);
}

## Route Aliases / Named Routes

These allow you to not have to change all links, when there is a change

```
Route::get('blog/category/{category}',
  ['as' => 'blog.category', 'uses' => 'BlogController@category']);
```

#### Referencing Routes

```
<a href="{{ URL::route('blog.category', ['category' => 'php'])}}">PHP</a>
```

## Listing all Routes from the Cli

```
php artisan route:list
```

_Note: You will need the database setup first_


## Caching Routes

Serialises route definitions into a single file (`bootstrap/cache/routes.php`), laravel no longer has to parse route definitions.

```
                                              php artisan route:cache
```

However if it is changed you have to run it again or to stop caching use:

```
php artisan route:clear
```

## Route Annotations

This feature caused some trouble in the community, so it was removed from laravel core.
To add it check the [github repo](https://github.com/LaravelCollective/annotations)

```
/**
 * @GET("/")
 */
 public function index(){
   return view('welcome');
 }
```

With parameters:

```
/**
 * @GET("/blog/category/{category}")
 */
```

With route aliases:

```
/**
 * @GET("/blog/category/{category}", as="blog.category")
 */
```
