[< Volver a la pagina principal](/docs/readme.md)

# Collection Sorting and Caching Refresher

Se sabe que cada publicación ahora incluye la fecha de publicación como parte de sus metadatos; sin embargo, el feed actualmente no está ordenado según esa fecha. Afortunadamente, debido a que usamos colecciones de Laravel, tareas como esta son muy fáciles. En este episodio, arreglaremos la clasificación y luego discutiremos el almacenamiento en caché "para siempre".

Basicamente arreglamos la funcion `all()` del archivo `Post.php`, ordenando los post mediante la fecha y además eliminando el cache:

```php
 public static function all()
    {
        return cache()->rememberForever('post.all', function () {
            return collect(File::files(resource_path("posts")))

                ->map(fn ($file) => YamlFrontMatter::parseFile($file))
                ->map(fn ($document) => new Post(
                    $document->title,
                    $document->excerpt,
                    $document->date,
                    $document->body(),
                    $document->slug
                ))
                ->sortByDesc('date');
        });
    }
```

Luego para comprobar lo del cache, abrimos nuestra maquina virtual y nos ubicamos en `vagrant@webserver:~/sites/lfts.isw811.xyz$`, y realizamos lo siguiente:

```bash
    php artisan tinker
```

Esto abre como una consola para ver revisar lo del cache.

Y luego con la siguiente linea de codigo podremos revisar la cache de la pagina:

```bash
    cache('post.all')
```