[< Volver a la pagina principal](/docs/readme.md)

# Route wildcard constraints

En este episodio, lo que haremos es agregar una restricción a nuestra ruta para garantizar que el slug de la publicación del blog consista exclusivamente en cualquier combinación de letras, números, guiones y guiones bajos.

```php
Route::get('posts/{post}', function ($slug) {

    $path = __DIR__ . "/../resources/posts/{$slug}.html";

    if (! file_exists($path)){
       return redirect('/');
    }

    $post = file_get_contents($path);


    return view('post', [
        'post' => $post
    ]);

})->where('post', '[A-z_\-]+');
```