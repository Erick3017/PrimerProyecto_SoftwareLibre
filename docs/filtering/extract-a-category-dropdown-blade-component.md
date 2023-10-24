[< Volver a la pagina principal](/docs/readme.md)

# Extract a Category Dropdown Blade Component

En este episodio, crearemos un componente desplegable dedicado de categoría x que podría ser responsable de obtener cualquier dato que el desplegable requiera.

Para comenzar, abrimos nuestra maquina virtual y ejecutamos el siguiente comando para crear un nuevo archivo llamado `category-dropdown.blade.php`.

```bash
php artisan make:component CategoryDropdown
```

Seguidamente, nos ubicamos en el archivo `_post-header.blade.php` y cortamos todo el código del componente `x-dropdown` y agregamos el siguiente componente.

```php
<x-category-dropdown />
```

Ahora, nos vamos al nuevo archivo creado `category-dropdown.blade.php` y pegamos el código cortado anteriormente.

```php
<x-dropdown>
    <x-slot name="trigger">
        <button class="py-2 pl-3 pr-9 text-sm font-semibold w-full lg:w-32 text-left flex lg:inline-flex">{{ isset($currentCategory) ? ucwords($currentCategory->name) : 'Categories' }}

            <x-icon name="down-arrow" class="absolute pointer-events-none" style="right: 12px;" />
        </button>
    </x-slot>


    <x-dropdown-item href="/" :active="request()->routeIs('home')">All</x-dropdown-item>


    @foreach ($categories as $category)
    <x-dropdown-item href="/?category={{ $category->slug }}" :active='request()->is("categories/{$category->slug}")'>{{ ucwords($category->name) }}</x-dropdown-item>
    @endforeach
</x-dropdown>
```

Ahora, nos vamos al archivo `CategoryDropdown.php` y modificamos el return, agregando lo siguiente.
, el `currentCategory` lo cortamos del archivo `PostController.php`
```php
return view('components.category-dropdown', [
            'categories' => Category::all(),
            'currentCategory' => Category::firstWhere('slug', request('category'))]);
```

Y ahora, nos vamos a los archivos `web.php` y `PostController.php` y eliminamos la siguiente linea de código de los return.

```php
'categories' => Category::all()
```

Continuando, creamos una carpeta llamada `posts` dentro de la carpeta `views`, y posteriormente movemos los archivos `_post-header.blade.php`, `post.blade.php` y `posts.blade.php`, ahora, una vez dentro de la nueva carpeta `posts` cambiamos los nombres de los archivos.

* Primero el archivo `_post-header.blade.php`, lo cambiamos a `_header.blade.php`.
* Luego, el archivo `post.blade.php`, lo cambiamos a `show.blade.php`.
* Y por ultimo, el archivo `posts.blade.php`, lo cambiamos a `index.blade.php`.

Y ahora en el archivo `index.blade.php` modificamos el `@include`.

```php
@include ('posts._header')
```

La pagina web se debería funcionar perfectamente.

![Verificando pagina](./images/verificar.png)



