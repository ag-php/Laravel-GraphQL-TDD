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

Publicar el archivo de configuración:
    ``` php artisan vendor:publish --provider=”Folklore\GraphQL\ServiceProvider” ```

Revisar la configuración:
En el archivo ‘config/graphql.php’ recién creado está a configuración de GraphQL.



## Creando casos de prueba (TEST CASE)
Para los casos de prueba se utilizará el plugins de terceros “kunicmarko/graphql-test”:
    ``` composer require kunicmarko/graphql-test ```

Se crea el archivo de prueba dentro de la carpeta ‘tests/Feature’:
    ``` php artisan make:test UserQueryTest ```
    

##### El archivo ‘UserQueryTest.php’ debe cumplir ciertas restricciones para poder usar el plugins de tercero:
* Heredar de “KunicMarko\GraphQLTest\Bridge\Laravel\TestCase” y usarla;
* Usar “KunicMarko\GraphQLTest\Operation\Query” o “KunicMarko\GraphQLTest\Operation\Mutation” según se necesite.
* En clase “TestCase” que se heredó, hay un método abstracto (createApplication) el cual hay que implemeter.
* Es necesario definir el tipo de cabecera que se utilizará.

La ruta del endpoint por defecto es ‘/graphql’, pero si se desea cambiarla se debe adicionar esto a la clase deseada:
 ``` public static $endpoint = ‘/’; ```
 
 
 