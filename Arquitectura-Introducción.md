Nuestra arquitectura esta basada en varias arquitecturas similares:

* Arquitectura Limpia
* Arquitectura Hexagonal
* Arquitectura Onion
* Adaptative Code (Microsoft)

Existen muchos recursos en la web y algunos libros si desean profundizar, nos basamos en estas arquitecturas por que prometen ayudarnos a cumplir con nuestros objetivos, puedes ver el repositorio de [GitHub](https://github.com/bambucode/arquitectura) para que sea más sencillo seguir esta documentación.

La arquitectura se compone de dos cosas, paquetes y objetos;

### Paquetes

![](https://raw.githubusercontent.com/bambucode/resources/36362b643cb93249b3e96fcccece66bfd7ebc41d/arquitectura_paquetes.svg)

Cada paquete es un conjunto de clases que están relacionadas o que hacen cosas similares , por ejemplo el paquete Database esta compuesto por muchas clases pero todas tienen la responsabilidad común de trabajar con la base de datos, en Delphi no existe el concepto de paquetes así es que simplemente las separamos usando carpetas, nuestra arquitectura se compone de los siguientes paquetes:

* Aplicación (namespace app)
* Dominio
* Infraestructura (namespace infra)
* Common
* User Interface (namespae ui)

y a su vez cada una de estos paquetes esta compuesta de otros paquetes o simplemente de folders que agrupan los objetos similares, cada uno de los paquetes tiene su propia sección y ahí veremos sus componentes.

### Objetos

Existen una serie de objetos con responsabilidades e interacciones ya definidas, los cuales veremos dentro de cada uno de los paquetes.

* Entidades
* Data Transfer Objects
* Repositorios
* Comandos
* Queries
* Servicios
* Mapeadores
* Excepciones
