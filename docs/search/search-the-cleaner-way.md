[< Volver a la pagina principal](/docs/readme.md)

# Search (The Cleaner Way)

Ahora que nuestro formulario de búsqueda está funcionando, vamos a refacturar el código en algo más agradable a la vista y aun mas reutilizable. 

En este episodio, no solo vamos a echar un vistazo a las clases de controlador, sino que también vamos a aprender sobre los ámbitos de consulta elocuentes.

Para comenzar, abrimos nuestra maquina virtual en la ubicación de `/vagrant/sites/lfts.isw811.xyz` y ejecutamos el comando para crear un archivo controller llamado `PostController`.

```bash
php artisan make:controller PostController
```

Ahora, nos ubicamos en el archivo `web.php` y cortamos el código que se encuentra dentro de la ruta `Route::get('/')` y también el que se encuentra dentro de la ruta `Route::get('posts/{post:slug}')`.

Seguidamente, nos vamos al nuevo archivo nuevo creado llamado `PostController.php`que se encuentra en la carpeta `Controller`, dentro de la carpeta `Http` y creamos dos funciones, una se llama `index()`, en esa copiamos el código anteriormente cortado de la ruta `Route::get('/')` y en la otra función llamada `show()` copiamos el código de la ruta `Route::get('posts/{post:slug}')`.

```php
<?php

namespace App\Http\Controllers;

use App\Models\Category;
use App\Models\Post;


use Illuminate\Http\Request;

class PostController extends Controller
{
    public function index()
    {
        $posts = Post::latest();

        if (request('search')) {
            $posts
                ->where('title', 'like', '%' . request('search') . '%')
                ->orWhere('body', 'like', '%' . request('search') . '%');
        }
        return view('posts', [
            'posts' => $posts->get(),
            'categories' => Category::all()
        ]);
    }

    public function show(Post $post)
    {
        return view('post', [
            'post' => $post
        ]);
    }
}
```

Volvemos al archivo `web.php` y agregamos lo siguiente a las rutas `Route::get('/')` y `Route::get('posts/{post:slug}'`:

```php
Route::get('/', [PostController::class, 'index'])->name('home');
```
```php
Route::get('posts/{post:slug}', [PostController::class, 'show']);
```

Ahora nos vamos al archivo `Post.php` y editamos la siguiente función.

```php
public function scopeFilter($query, array $filters)
    {
        $query->when($filters['search'] ?? false, fn ($query, $search) =>
            $query
                ->where('title', 'like', '%' . $search . '%')
                ->orWhere('body', 'like', '%' . $search . '%'));
    }
```

Y para finalizar, volvemos al archivo `PostController.php` y editamos la función `index()`.

```php
public function index()
    {
        return view('posts', [
            'posts' => Post::latest()->filter(request(['search']))->get(),
            'categories' => Category::all()
        ]);
    }
```


