El propósito de este documento es dar a conocer las practicas que debemos seguir al desarrollar eleventa desktop.

## Objetivo

Nuestro objetivo principal como equipo de desarrollo es fabricar un producto de software de calidad que cuente con las siguientes 3 características:

1. ### Que sea fácil de entender

La facilidad de análisis permite que los nuevos programadores logren ser productivos mas rápidamente, incluso los mas experimentados se benefician de esta característica, podemos decir que a mayor dificultad de código mayor es el tiempo necesario para implementar cambios.
 
2. ### Que sea fácil de modificar

Existen 3 tipos de modificaciones: Corrección de bugs, nuevos features y mejoras, debemos desarrollar el software de una manera que permita llevar a cabo esas modificaciones de la manera mas sencilla y rápida posible.

3. ### Que sea fácil de probar

Para asegurar que el software cumple con los requerimientos necesarios y que no tiene fallas(o tiene un numero pequeño) debemos probar cada parte del software, si el software no se desarrolla pensando en que debe ser probado puede hacer muy difícil el proceso de pruebas.

## Como lo haremos?

Para lograr cumplir los objetivos antes mencionados tenemos algunas herramientas:

1. ### Arquitectura

La arquitectura hace mas facil de entender el código ya que establece que tipos de objetos existen, cual es su responsabilidad y donde deben residir(en que layer, modulo , folder , etc.), así como tambien la manera en que los objetos deben interactuar, al no existir una arquitectura clara y documentada puede parecer que el código carece de una estructura definida lo que hace mas difícil entenderlo y modificarlo.

2. ### Convenciones

Las convenciones nos ayudan a escribir código mas claro y por lo tanto mas facil de entender, un ejemplo claro es la nomenclatura de variables:

```pascal
procedure Agregar(const objeto: TProd);
var
  r: TRes;
begin
  r := rep.Save(objeto);
  if r.Succ then
    rep.Trans(r);
end;

//Versus

procedure AgregarProducto(const aProducto: TProducto);
var
  response: TResponse;
begin
  response := repositoryProducto.Agregar(aProducto);
  
  if response.Success then
    repository.RegistrarTransaccion(response);
end;
```
