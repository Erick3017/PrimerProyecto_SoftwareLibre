[< Volver a la pagina principal](/docs/readme.md)

# Blade: The Absolute Basics

Blade es el motor de plantillas de Laravel para tus vistas. Puede considerarlo como una capa encima de PHP para hacer que la sintaxis sea requerida para construir estas vistas, y estas sean lo más limpia y concisa posible. En última instancia, estas plantillas de Blade serán compiladas para vainilla PHP detrás de escena.

Un ejemplo es cambiar el archivo `post.blade.php` a `post.php`, y ealizar el siguiente cambio en el código:

```html
 <body>
    <?php foreach ($posts as $post) : ?>
        <article>
            <h1>
                <a href="/posts/<?= $post->slug; ?>">
                    {{ $post->title }}
                </a>

            </h1>

            <div>
                <?= $post->excerpt; ?>
            </div>
        </article>
    <?php endforeach; ?>
</body>
```

Si el archivo esta con la extension `.blade` la pagina debería funcionar sin ningun problema:

![Pagina con la extension .blade](images/con_el_.blade.png)

En cambio, si el archivo no tiene la extensión `.blade` y se llama simplemente `post.php`, la página no mostrará los títulos de los post.

![Pagina sin la extension .blade](images/sin_el_.blade.png)

A continuación, para seguir utilizando la sintaxis de Blade correctamente, modificaremos el archivo `posts.blade.php`, cambiando el bucle `@foreach`, la forma en que se acceden a los atributos y agregando una clase al artículo:

 ```html
 <body>
    @foreach ($posts as $post)
    <article class="{{ $loop->even ? 'foobar' : '' }}">
        <h1>
            <a href="/posts/{{ $post->slug }}">
                {{ $post->title }}
            </a>

        </h1>

        <div>
            {{ $post->excerpt }}
        </div>
    </article>
    @endforeach
</body>
```

De igual manera, modificamos el archivo `post.blade.php`:

 ```html
 <body>
    <article>
        <h1>{{ $post->title }}</h1>

        <div>
            {!! $post->body !!}
        </div>
    </article>

    <a href="/">Go back</a>
</body>
```

