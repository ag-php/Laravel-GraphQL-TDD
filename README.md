# Laravel-GraphQL-TDD

Ejemplo de como usar la técnica de desarrollo guiado por pruebas en un API con GraphQL sobre Laravel.


# Casos de prueba para GraphQL en Laravel

## Requisitos
    Laravel^5.6, php^7.1
    
## Instalando GraphQL
Abre un terminal en la carpeta de tu proyecto y tecle:
    ``` Composer require folklore/graphql ```
    
## Configurando GraphQL
Agregar el ‘service provider’ al archivo ‘config/app.php’:
    ``` Folklore\GraphQL\ServiceProvider::class, ```

Agregar el ‘Facade’ al archivo ‘config/app.php’:
    ``` ‘GraphQL’ => Folklore\GraphQL\Support\Facades\GraphQL::class, ```

### Publicar el archivo de configuración:
    ``` php artisan vendor:publish –provider=”Folklore\GraphQL\ServiceProvider” ```

### Revisar la configuración:
En el archivo ‘config/graphql.php’ recién creado está a configuración de GraphQL.

