[< Volver a la pagina principal](/docs/readme.md)

# Eager Load Relationships on an Existing Model

En este episodio, lo que haremos será especificar qué relaciones deben cargarse de forma predeterminada en un modelo.

Iniciamos abriendo el browser para crear post con el siguiente comando:

```bash
php artisan tinker
```

Seguidamente, ya dentro de ese browser ejecutamos el siguiente comando para crear 10 posts asociados solamente a la categoría con el id numero 1. 

```bash
App\Models\Post::factory(10)->create(['category_id' => 1]);
```

Ahora nos ubicaremos en el archivo `web.php` y modicaremos las rutas para renderizar mejor la pagina con la siguiente linea de código, esto eso tanto en la ruta slug, como en la de username.

```php
'posts' => $category->posts->load(['category', 'author' ])
```

Ahora bien, para hacer el código mas clean y ahorrarnos los dos anteriores pasos, nos vamos para el archivo `Post.php` y agregamos la siguiente linea de código que se hace exactamente lo mismos.

```php
protected $with = ['category', 'author'];
```



