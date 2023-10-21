[< Volver a la pagina principal](/docs/readme.md)

# How to Extract a Dropdown Blade Component

En el episodio anterior hemos construido con éxito la funcionalidad básica para un menú desplegable, pero aún no es reutilizable. 

Para remediar esto, en este episodio vamos a extraer un componente X-Dropdown Blade. Esto vendrá con el efecto secundario de aislar todo el código específico de Alpine a ese archivo de componente único.

Para comenzar, vamos a crear un nuevo archivo llamado `dropdown.blade.php` dentro de la carpeta `components` y le adjuntamos el siguiente código:

```php
@props (['trigger'])

<div x-data="{show: false}" @click.away="show = false">
    <!-- Trigger  -->

    <div @click="show = ! show">
        {{ $trigger }}
    </div>

   <!-- Links -->
    <div x-show="show" class="py-2 absolute bg-gray-100 mt-2 rounded-xl w-full z-50" style="display: none">
        {{ $slot }}
    </div>
</div>
```

Después, nos vamos al archivo `_post-header.blade.php ` y modificamos el siguientes `div`.

```html
<div class="space-y-2 lg:space-y-0 lg:space-x-4 mt-8">
    <!--  Category -->
    <div class="relative lg:inline-flex bg-gray-100 rounded-xl">
        <x-dropdown>
            <x-slot name="trigger">
                <button class="py-2 pl-3 pr-9 text-sm font-semibold w-full lg:w-32 text-left flex lg:inline-flex">{{ isset($currentCategory) ? ucwords($currentCategory->name) :'Categories' }}

                    <svg class="transform -rotate-90 absolute pointer-events-none" style="right: 12px;" width="22" height="22" viewBox="0 0 22 22">
                        <g fill="none" fill-rule="evenodd">
                            <path stroke="#000" stroke-opacity=".012" stroke-width=".5" d="M21 1v20.16H.84V1z">
                            </path>
                            <path fill="#222" d="M13.854 7.224l-3.847 3.856 3.847 3.856-1.184 1.184-5.04-5.04 5.04-5.04z"></path>
                        </g>
                    </svg>
                </button>
            </x-slot>


            <a href="/" class="block text-left px-3 text-sm leading-6 hover:bg-blue-500 focus:bg-blue-500 hover:text-white focus:text-white">All</a>


            @foreach ($categories as $category)
            <a href="/categories/{{ $category->slug }}" class="block text-left px-3 text-sm leading-6 hover:bg-blue-500 focus:bg-blue-500 hover:text-white focus:text-white {{ isset($currentCategory) && $currentCategory->is($category) ? 'bg-blue-500 text-white' : ''}}">{{ ucwords( $category->name )}}</a>
            @endforeach
        </x-dropdown>
    </div>
```

Ahora creamos los siguiente archivos `dropdown-item.blade`, `dropdown.blade.php` y `icon.blade.php`.

* Primero en el archivo `dropdown-item.blade.php` agregamos lo siguiente:

```php
@props(['active' => false])

@php
    $classes = 'block text-left px-3 text-sm leading-6 hover:bg-blue-500 focus:bg-blue-500 hover:text-white focus:text-white';

    if ($active) $classes .= ' bg-blue-500 text-white';
@endphp

<a {{ $attributes(['class' => $classes]) }}
>{{ $slot }}</a>
```

* Seguidamente, en el archivo `dropdown.blade.php` agregamos lo siguiente:

```php
@props (['trigger'])

<div x-data="{show: false}" @click.away="show = false">
    <!-- Trigger  -->

    <div @click="show = ! show">
        {{ $trigger }}
    </div>

   <!-- Links -->
    <div x-show="show" class="py-2 absolute bg-gray-100 mt-2 rounded-xl w-full z-50" style="display: none">
        {{ $slot }}
    </div>
</div>
```

Y por ultimo el archivo `icon.blade.php` agregamos lo siguiente, este archivo es para manejar la flecha para abajo del menu desplegable.

```php
@props(['name'])

@if ($name == 'down-arrow')
    <svg {{ $attributes(['class' => 'transform -rotate-90']) }} width="22" height="22" viewBox="0 0 22 22">
        <g fill="none" fill-rule="evenodd">
            <path stroke="#000" stroke-opacity=".012" stroke-width=".5" d="M21 1v20.16H.84V1z">
            </path>
            <path fill="#222" d="M13.854 7.224l-3.847 3.856 3.847 3.856-1.184 1.184-5.04-5.04 5.04-5.04z"></path>
        </g>
    </svg>
@endif
```

Y ya para finalizar editamos el archivo `web.php` para agregar lo siguiente a las rutas en la ruta `/` y tambien en la ruta `categories`:

```php
Route::get('/', function () {

    return view('posts', [
        'posts' => Post::latest()->get(),
        'categories' => Category::all()
    ]);
})->name('home');
```

```php
Route::get('categories/{category:slug}', function (Category $category) {

    return view('posts', [
        'posts' => $category->posts,
        'currentCategory' => $category,
        'categories' => Category::all()
    ]);
})->name('category');
```