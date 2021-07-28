Aunque formalmente no usamos CQRS (Command Query Responsability Segregation) si usamos la forma mas basica de separación , nuestra arquitectura puede verse de la siguiente manera:

![](https://raw.githubusercontent.com/bambucode/resources/3b9df4b468c373f603f34ceeecb7f093ec1b9a11/cqrs2.svg)

La interface de usuario puede realizar queries o ejecutar comandos, la diferencia entre ambos es que los comandos utilizan al dominio y su lógica siempre es mas compleja, por otro lado los queries simplemente obtienen datos almacenados en la base de datos.

Debido a esta diferencia entre comandos y queries tambien se produce una diferencia en los repositorios, los repositorios normales que son los que usan los comandos trabajan siempre sobre entidades de dominio, por el contrario los repositorios read only o repositorios de query utilizan DTO y solo pueden hacer lecturas, esta diferencia es por que usar entidades de dominio para luego solo mapearleas a DTOs no tendría mucho sentido aunque es una opción valida, otra opción seria que la interface de usuario accediera directamente al repositorio read only pero recordemos que en el Query se hacen cosas adicionales como la validación de privilegios.

Antes comentamos que los comandos son siempre mas complejos que los queries, en los repositorios se invierte esta regla, los repositorios que usan los comandos generalmente son muy sencillos, solo realizan las 4 operaciones básicas CRUD y un poco mas, en el caso de los repositorios de lectura estos son mas complejos por que aquí se hacen las consultas mas elaboradas, por ejemplo un reporte que puede implicar queries SQL muy grandes.
