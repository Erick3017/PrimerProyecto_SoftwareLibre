[< Volver a la pagina principal](/docs/readme.md)

# Fix a Confusing Eloquent Query Bug

Parece que tenemos un ligero error dentro de nuestro ámbito de consulta filter(). 

En este episodio, revisaremos la consulta SQL subyacente que está produciendo los resultados incorrectos, y luego arreglaremos el error en nuestra consulta elocuente.

Lo que haremos solamente es irnos al archivo `Post.php` y modificar en la función `scopeFilter` el filtro de `search`.

```php
 $query->when($filters['search'] ?? false, fn ($query, $search) =>
        $query->where(fn($query)=>
            $query->where('title', 'like', '%' . $search . '%')
            ->orWhere('body', 'like', '%' . $search . '%')));
```

Y ahora nos vamos a la pagina a verificar que todo este correcto.

![Verificar pagina](./images/busquedafinish.png)