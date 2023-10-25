[< Volver a la pagina principal](/docs/readme.md)

# Laravel Breeze Quick Peek

Debido a que ya hemos pasado algún tiempo trabajando en un sistema de autenticación personalizado para nuestro blog, seguiremos adelante.

Sin embargo, todavía debemos dejar de lado unos momentos para revisar rápidamente los excelentes paquetes de autenticación de primera mano de Laravel. Específicamente, en este episodio, echaremos un vistazo a Laravel Breeze.

En este episodio, lo que se realiza es un nuevo proyecto para ver lo de Laravel.

No obstante, modificamos el `if` y el `return` del archivo `SessionController.php` de la siguiente manera.

```php
if (! auth()->attempt($attributes)) {
            throw ValidationException::withMessages([
                'email' => 'Your provided credentials could not be verified.'
            ]);
        }

        session()->regenerate();

        return redirect('/')->with('success', 'Welcome Back!');
```

