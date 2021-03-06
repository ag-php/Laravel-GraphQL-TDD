# Laravel-GraphQL-TDD

Ejemplo de como usar la técnica de desarrollo guiado por pruebas en un API con GraphQL sobre Laravel.


# Casos de prueba para GraphQL en Laravel

## Requisitos
    Laravel^5.4, php^7.1
    
## Instalando GraphQL
Abre un terminal en la carpeta de tu proyecto y teclee:
    ``` composer require folklore/graphql ```
    
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
* Heredar de “KunicMarko\GraphQLTest\Bridge\Laravel\TestCase” y usarla.
* Usar “KunicMarko\GraphQLTest\Operation\Query” o “KunicMarko\GraphQLTest\Operation\Mutation” según se necesite.
* En la clase “TestCase” que se heredó, hay un método abstracto (createApplication) el cual hay que implemetar.
* Es necesario definir el tipo de cabecera que se utilizará.

La ruta del endpoint por defecto es ‘/graphql’, pero si se desea cambiarla se debe adicionar esto a la clase deseada:
 ``` public static $endpoint = ‘/’; ```
 

## Métodos de ejemplo (QUERY)
```php
    use Illuminate\Support\Str;
    use KunicMarko\GraphQLTest\Bridge\Laravel\TestCase;
    use KunicMarko\GraphQLTest\Operation\Query;
```

```php
    protected function setUp()
    {
        $this->setDefaultHeaders([
            'Content-Type' => 'application/json',
        ]);
    }
```

```php
   public function testUserQuery(): void
    {
        $query = $this->query(
            new Query(
                'user',
                [
                    'id' => 1
                ],
                [
                    'id',
                    'name',
                    'email',
                    'password'
                ]
            )
        );

        $actual = json_encode($query);
        $expectedUserNull = json_encode(["user" => null]);
        $expectedErrors = json_encode("errors");

        if(Str::contains($actual, $expectedUserNull))
        {
            $this->assertTrue(false, "El usuario no se ha mostrado, usuario null");
        }
        elseif (Str::contains($actual, $expectedErrors))
        {
            $this->assertTrue(false, "El usuario no se ha mostrado, existen errores en los datos");
        }
        else{
            $this->assertTrue(true, "El usuario se ha mostrado correctamente");
        }
    } 
    
```

## Métodos de ejemplo (MUTATIONS)
```php
    use Illuminate\Support\Str;
    use KunicMarko\GraphQLTest\Bridge\Laravel\TestCase;
    use KunicMarko\GraphQLTest\Operation\Mutation;
    use Faker\Factory as Faker;
```
    
```php
    protected function setUp()
    {
        $this->setDefaultHeaders([
            'Content-Type' => 'application/json',
        ]);
    }
```

```php
    public function testCreateUserMutation():void
    {
        $faker = Faker::create();
        $mutation = $this->mutation(
            new Mutation(
                'createUser',
                [
                    'name' => $faker->name(),
                    'email' => $faker->unique()->email(),
                    'password' => $faker->password()
                ],
                [
                    'id',
                    'name',
                    'email',
                    'password'
                ]
            )
        );

        $actual = json_encode($mutation);
        $expectedUserNull = json_encode(["createUser" => null]);
        $expectedErrors = json_encode("errors");

        if(Str::contains($actual, $expectedUserNull))
        {
            $this->assertTrue(false, "El usuario no se ha creado, usuario null");
        }
        elseif (Str::contains($actual, $expectedErrors))
        {
            $this->assertTrue(false, "El usuario no se ha creado, existen errores en los datos");
        }
        else{
            $this->assertTrue(true, "El usuario se ha creado correctamente");
        }
    }
```


Para ejecutar los casos de prueba abrir el terminal en la carpeta del proyecto y teclear la siguiente linea:
``` vendor/bin/phpunits test/Feature/UsersQueryTest.php ```
 
 
## NOTA: 
### Si al ejecutar un test se muestra el error:
“Call to a member function make() on null”

Ir al archivo ``` ‘vendor/laravel/framework/src/Illuminate/Fundation/Testing/Concerns/MakesHttpRequests.php’``` y cambiar la siguiente linea:
```php 
    $kernel = $this->app->make(HttpKernel::class); 
```
por esta otra:
```php 
    $kernel = $this->createApplication()->make(HttpKernel::class);
```

