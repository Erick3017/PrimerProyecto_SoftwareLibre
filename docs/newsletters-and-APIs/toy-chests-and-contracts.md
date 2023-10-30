[< Volver a la pagina principal](/docs/readme.md)

# Toy Chests and Contracts

Este próximo episodio es suplementario. Es un poco más avanzado y revisa los contenedores de servicios, proveedores y contratos.

Iniciamos en el archivo `AppServiceProvider.php` agregando el siguiente código en la función `register()`.7

```php
app()->bind(Newsletter::class, function () {

            $client = (new ApiClient)->setConfig([
                'apiKey' => config('services.mailchimp.key'),
                'server' => 'us6'
            ]);


            return new Newsletter($client);
        });
```

Después, nos vamos al archivo `Newsletter.php` y eliminamos la función `client()`

Seguidamente, le cambiamos el nombre al archivo `Newsletter.php` por `MailchimpNewsletter.php`.

Además, creamos dos archivos nuevos en la carpeta de `Services` llamados `ConvertKitNewsletter.php` y `Newsletter.php`

Y volvemos al archivo `MailchimpNewsletter.php` y le implementamos a la clase el archivo `Newsletter.php`.

```php
class MailchimpNewsletter implements Newsletter
```

Ahora primero, nos vamos al archivo nuevo creado `Newsletter.php` y agregamos lo siguiente.

```php
<?php

namespace App\Services;


interface Newsletter
{
    public function subscribe (string $email, string $list = null);
}
```

Y después, nos vamos al archivo `ConvertKitNewsletter.php` y agregamos lo siguiente.

```php
<?php

namespace App\Services;

use MailchimpMarketing\ApiClient;

class ConvertKitNewsletter implements Newsletter
{

    public function subscribe(string $email, string $list = null)
    {
    
    }
}

```

Y para finalizar, nos vamos al archivo `AppServiceProvider.php` y editamos el return.

```php
 return new MailchimpNewsletter($client);
```

