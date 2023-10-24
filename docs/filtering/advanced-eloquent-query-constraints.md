[< Volver a la pagina principal](/docs/readme.md)

# Advanced Eloquent Query Constraints

En este episodio vamos a continuar con la consulta filter() de nuestro modelo Post. Tal vez podamos filtrar adicionalmente los mensajes según su categoría. 

Si tomamos este enfoque, tendremos una poderosa manera de combinar filtros.

Para empezar, nos ubicamos en el archivo `PostController.php` y agregamos `category` en el request del return de `posts` y también agregamos el `currentCategory`.

```php
return view('posts', [
            'posts' => Post::latest()->filter(request(['search', 'category']))->get(),
            'categories' => Category::all()
            'currentCategory' => Category::firstWhere('slug', request('category'))
        ]);
```

Seguidamente, nos vamos al archivo `Post.php` y agregamos lo siguiente en la función `scopeFilter` para buscar por categoría.

```php
$query->when($filters['category'] ?? false, fn($query, $category) =>
        $query->whereHas('category', fn ($query) =>
        $query->where('slug', $category)));
```

Posteriormente, nos vamos al archivo `web.php` y eliminamos la ruta `categories`.

Después, nos vamos al archivo `_post-header.blade.php` y modificamos el `href` del componente `<x-dropdown-item`.

```php
<x-dropdown-item href="/?category={{ $category->slug }}" :active='request()->is("categories/{$category->slug}")'>{{ ucwords($category->name) }}</x-dropdown-item>
```